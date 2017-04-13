<properties
    pageTitle="Azure AD B2C: Ασφαλή web API χρησιμοποιώντας Node.js | Microsoft Azure"
    description="Πώς να δημιουργείτε μια τοποθεσία web Node.js API που δέχεται διακριτικά από ένα μισθωτή B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Ασφαλή web API χρησιμοποιώντας Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Με B2C Azure Active Directory (Azure AD), μπορείτε να ασφαλίσετε μια API web με χρήση διακριτικά πρόσβασης OAuth 2.0. Αυτά τα διακριτικά επιτρέπει τις εφαρμογές προγράμματος-πελάτη που χρησιμοποιούν Azure AD B2C για τον έλεγχο ταυτότητας για το API. Αυτό το άρθρο σας δείχνει πώς μπορείτε να δημιουργήσετε μια "λίστα εκκρεμών εργασιών" API που επιτρέπει στους χρήστες για να προσθέσετε και λίστα εργασίες. Το web API είναι ασφαλή χρήση Azure AD B2C και επιτρέπει μόνο εξουσιοδοτημένους χρήστες για να διαχειριστείτε τη λίστα εκκρεμών εργασιών.

> [AZURE.NOTE]  Αυτό το δείγμα έχει συνταχθεί να είστε συνδεδεμένοι με τη χρήση μας [iOS B2C δείγμα εφαρμογής](active-directory-b2c-devquickstarts-ios.md). Κάντε πρώτα την τρέχουσα Γνωρίστε και, στη συνέχεια, να ακολουθήσετε μαζί με το δείγμα.

**Passport** είναι το ενδιάμεσο ελέγχου ταυτότητας για Node.js. Ευέλικτη και modular, Passport μπορεί να εγκατασταθεί unobtrusively σε οποιαδήποτε επείγοντα ή Restify εφαρμογής web. Ένα ολοκληρωμένο σύνολο στρατηγικές υποστηρίζει έλεγχο ταυτότητας με χρήση του ονόματος χρήστη και τον κωδικό πρόσβασης, Facebook, Twitter και πολλά άλλα. Έχουμε αναπτύξει μια στρατηγική για Azure Active Directory (Azure AD). Εγκαταστήσετε αυτήν τη λειτουργική μονάδα και, στη συνέχεια, να προσθέσετε το Azure AD `passport-azure-ad` προσθήκης.

Για να το κάνετε αυτό το δείγμα, πρέπει να:

1. Καταχωρήστε μια εφαρμογή του Azure AD.
2. Ρύθμιση της εφαρμογής σας για χρήση του Passport `azure-ad-passport` προσθήκης.
3. Ρύθμιση παραμέτρων μιας εφαρμογής προγράμματος-πελάτη για να καλέσετε το API web "λίστα εκκρεμών εργασιών".


## <a name="get-an-azure-ad-b2c-directory"></a>Λήψη ενός καταλόγου Azure AD B2C

Μπορείτε να χρησιμοποιήσετε Azure AD B2C, πρέπει να δημιουργήσετε έναν κατάλογο, ή να μισθωτή.  Ένας κατάλογος είναι ένα κοντέινερ για όλους τους χρήστες, εφαρμογές, ομάδες και πολλά άλλα.  Ήδη, εάν δεν έχετε ένα [Δημιουργία καταλόγου B2C](active-directory-b2c-get-started.md) πριν να συνεχίσετε.

## <a name="create-an-application"></a>Δημιουργία μιας εφαρμογής

Στη συνέχεια, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογό σας B2C που θα σας δώσει Azure AD ορισμένες πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας. Σε αυτήν την περίπτωση, η εφαρμογή προγράμματος-πελάτη και το web API αντιπροσωπεύονται από ένα μεμονωμένο **Αναγνωριστικό εφαρμογής**, επειδή αυτά περιλαμβάνουν μία λογική εφαρμογής. Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-b2c-app-registration.md). Βεβαιωθείτε ότι:

- Συμπεριλάβετε μια **στο web app/web api** στην εφαρμογή
- Πληκτρολογήστε `http://localhost/TodoListService` ως **διεύθυνση URL απάντηση**. Είναι η προεπιλεγμένη διεύθυνση URL για αυτό το δείγμα κώδικα.
- Δημιουργία μιας **εφαρμογής μυστικό** για την εφαρμογή σας και αντιγράψτε την. Χρειάζεστε αυτά τα δεδομένα αργότερα. Σημειώστε ότι αυτή η τιμή πρέπει να είναι [διαφυγής XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) πριν να το χρησιμοποιήσετε.
- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας. Χρειάζεστε αυτά τα δεδομένα αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Δημιουργία πολιτικών σας

Στο Azure AD B2C, κάθε εμπειρία χρήστη ορίζεται από μια [πολιτική](active-directory-b2c-reference-policies.md). Αυτή η εφαρμογή περιέχει δύο εμπειρίες ταυτότητας: εγγραφή στο και πραγματοποιήστε είσοδο. Πρέπει να δημιουργήσετε μία πολιτική κάθε τύπο, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Όταν δημιουργείτε τις πολιτικές τρεις σας, βεβαιωθείτε ότι:

- Επιλέξτε το **εμφανιζόμενο όνομα** και άλλα χαρακτηριστικά εγγραφής από την πολιτική της εγγραφής σας.
- Επιλέξτε το **εμφανιζόμενο όνομα** και το **Αναγνωριστικό αντικειμένου** δηλώσεις εφαρμογής σε κάθε πολιτική.  Μπορείτε να επιλέξετε καθώς και άλλες αξιώσεων.
- Αντιγραφή προς τα κάτω στο **όνομα** κάθε πολιτική μετά τη δημιουργία του. Θα πρέπει να έχει το πρόθεμα `b2c_1_`.  Χρειάζεστε αυτά τα ονόματα πολιτικής αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Αφού δημιουργήσετε τρεις πολιτικές σας, είστε έτοιμοι να δημιουργήσετε την εφαρμογή σας.

Για να μάθετε πώς λειτουργούν οι πολιτικές στο Azure AD B2C, ξεκινήστε με το [.NET web app ξεκινήσατε το πρόγραμμα εκμάθησης](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Κάντε λήψη του κώδικα

Ο κωδικός για αυτό προγραμμάτων εκμάθησης [διατηρούνται με GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Για να δημιουργήσετε το δείγμα καθώς εργάζεστε, μπορείτε να [κάνετε λήψη ενός έργου σκελετό ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Μπορείτε επίσης να αντιγράψετε το σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Η εφαρμογή ολοκληρωμένων είναι επίσης [διαθέσιμο ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) ή σε το `complete` υποκατάστημα στο ίδιο αποθετήριο.

## <a name="download-nodejs-for-your-platform"></a>Λήψη Node.js για την πλατφόρμα σας

Για να χρησιμοποιήσετε με επιτυχία αυτό το δείγμα, χρειάζεστε μια εγκατάσταση εργασία του Node.js. 

Εγκαταστήστε Node.js από [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Εγκατάσταση MongoDB για την πλατφόρμα σας

Για να χρησιμοποιήσετε με επιτυχία αυτό το δείγμα, χρειάζεστε μια εγκατάσταση εργασία του MongoDB. Χρησιμοποιούμε MongoDB για να κάνετε το REST API μόνιμη όλων των παρουσιών διακομιστή.

Εγκαταστήστε MongoDB από [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Γνωρίστε αυτό προϋποθέτει ότι χρησιμοποιείτε τα τελικά σημεία εγκατάστασης και server προεπιλογή για MongoDB, δηλαδή την ώρα της σύνταξης αυτού `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Εγκαταστήστε τις λειτουργικές μονάδες Restify της τοποθεσίας σας web API

Χρησιμοποιούμε Restify για να δημιουργήσετε το REST API. Restify ένα ελάχιστο και ευέλικτη πλαίσιο εφαρμογή Node.js προέρχεται από Express. Έχει ένα ισχυρό σύνολο δυνατοτήτων για τη δημιουργία APIs ΥΠΌΛΟΙΠΑ επάνω σε σύνδεση.

### <a name="install-restify"></a>Εγκατάσταση Restify

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`. Εάν το `azuread` καταλόγου δεν υπάρχει, δημιουργήστε το.

`cd azuread`ή`mkdir azuread;`

Εισαγάγετε την ακόλουθη εντολή:

`npm install restify`

Αυτή η εντολή εγκαθιστά Restify.

#### <a name="did-you-get-an-error"></a>Λάβατε ένα μήνυμα σφάλματος;

Σε ορισμένα λειτουργικά συστήματα, όταν χρησιμοποιείτε το `npm`, ενδέχεται να εμφανιστεί το σφάλμα `Error: EPERM, chmod '/usr/local/bin/..'` και αίτησης για να εκτελέσετε το λογαριασμό με δικαιώματα διαχειριστή. Εάν αυτό το πρόβλημα παρουσιάζεται, χρησιμοποιήστε το `sudo` εντολή για να εκτελέσετε `npm` σε υψηλότερο επίπεδο δικαιωμάτων.

#### <a name="did-you-get-a-dtrace-error"></a>Λάβατε ένα σφάλμα DTrace;

Μπορείτε να δείτε κάτι παρόμοιο με αυτό το κείμενο κατά την εγκατάσταση του Restify:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify παρέχει μηχανισμό ισχυρή για την ανίχνευση ΥΠΌΛΟΙΠΑ κλήσεις χρησιμοποιώντας DTrace. Ωστόσο, πολλά λειτουργικά συστήματα δεν έχουν DTrace διαθέσιμη. Μπορείτε να αγνοήσετε αυτά τα σφάλματα.

Το αποτέλεσμα της εντολής θα πρέπει να είναι παρόμοιο με αυτό το κείμενο:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Εγκατάσταση Passport της τοποθεσίας σας web API

Από τη γραμμή εντολών, αλλάξτε τον κατάλογό σας για να `azuread`, εάν δεν υπάρχει ήδη.

Εγκαταστήστε το Passport χρησιμοποιώντας την ακόλουθη εντολή:

`npm install passport`

Το αποτέλεσμα της εντολής πρέπει να μοιάζει με αυτό το κείμενο:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Προσθήκη passport azuread στην τοποθεσία web API

Στη συνέχεια, προσθέστε τη στρατηγική διακριτικό χρησιμοποιώντας `passport-azuread`, μιας οικογένειας στρατηγικές που συνδέουν Azure AD με Passport. Χρησιμοποιήστε αυτήν τη στρατηγική για τα διακριτικά φορέα στο δείγμα REST API.

> [AZURE.NOTE] Παρόλο που OAuth2 παρέχει ένα πλαίσιο στο οποίο μπορεί να εκδοθεί γνωστού διακριτικού τύπου, μόνο ορισμένων τύπων διακριτικού έχουν που αποκτήθηκε ευρεία χρήση. Τα διακριτικά για την προστασία των τελικά σημεία είναι τα διακριτικά φορέα. Αυτοί οι τύποι των κωδικών είναι η ευρύτερα έχει εκδοθεί στο OAuth2. Πολλές υλοποιήσεις λαμβάνεται ως δεδομένο ότι τα διακριτικά φορέα είναι ο μόνος τύπος διακριτικού εκδοθεί.

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη.

Εγκαταστήστε το διαβατήριο `passport-azure-ad` λειτουργική μονάδα χρησιμοποιώντας την ακόλουθη εντολή:

`npm install passport-azure-ad`

Το αποτέλεσμα της εντολής πρέπει να μοιάζει με αυτό το κείμενο:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Προσθήκη λειτουργικές μονάδες MongoDB στην τοποθεσία web API

Αυτό το δείγμα χρησιμοποιεί MongoDB ως χώρος αποθήκευσης δεδομένων. Για που εγκατάσταση Mongoose, μια χρησιμοποιείται ευρέως προσθήκης για τη Διαχείριση μοντέλων και οι διατάξεις.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Εγκατάσταση πρόσθετες λειτουργικές μονάδες

Στη συνέχεια, εγκαταστήστε τις υπόλοιπες απαιτούμενες λειτουργικές μονάδες.

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη:

`cd azuread`

Εγκαταστήστε τις λειτουργικές μονάδες στη σας `node_modules` καταλόγου:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Δημιουργήστε ένα αρχείο server.js με τις εξαρτήσεις

Το `server.js` αρχείο παρέχει το μεγαλύτερο μέρος των τη λειτουργικότητα για το διακομιστή Web API. 

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη:

`cd azuread`

Δημιουργία μιας `server.js` αρχείο σε ένα πρόγραμμα επεξεργασίας. Προσθέστε τις ακόλουθες πληροφορίες:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Αποθηκεύστε το αρχείο. Μπορείτε να επιστρέψετε σε αυτό αργότερα.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Δημιουργία αρχείου config.js για την αποθήκευση των ρυθμίσεων Azure AD

Αυτό το αρχείο κώδικα μεταβιβάζει τη ρύθμιση παραμέτρων από την πύλη Azure AD για να το `Passport.js` αρχείου. Δημιουργήσατε αυτές τις τιμές παραμέτρων όταν προσθέσατε το API web με την πύλη στο πρώτο μέρος του Γνωρίστε το. Θα σας εξηγούν τι μπορείτε να τοποθετήσετε τις τιμές αυτές τις παραμέτρους μετά την αντιγραφή του κώδικα.

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη:

`cd azuread`

Δημιουργία μιας `config.js` αρχείο σε ένα πρόγραμμα επεξεργασίας. Προσθέστε τις ακόλουθες πληροφορίες:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Απαιτούμενες τιμές

`clientID`: Το Αναγνωριστικό πελάτη της εφαρμογής σας Web API.

`IdentityMetadata`: Αυτό είναι το σημείο όπου `passport-azure-ad` αναζητά την υπηρεσία παροχής ταυτότητας για τα δεδομένα σας ρύθμισης παραμέτρων. Πραγματοποιεί αναζήτηση επίσης για τα πλήκτρα για να επικυρώσει τα διακριτικά web JSON. 

`audience`: Το ομοιόμορφο αναγνωριστικό πόρου (URI) από την πύλη που προσδιορίζει την εφαρμογή κλήσης. 

`tenantName`: Το όνομα του μισθωτή (για παράδειγμα, **contoso.onmicrosoft.com**).

`policyName`: Η πολιτική που θέλετε να επικυρώσετε τα διακριτικά που προέρχονται από το διακομιστή. Αυτή η πολιτική πρέπει να είναι η ίδια πολιτική που χρησιμοποιείτε για την εφαρμογή υπολογιστή-πελάτη για είσοδο.

> [AZURE.NOTE] Για την προεπισκόπηση B2C, χρησιμοποιήστε τις ίδιες πολιτικές κατά μήκος ρύθμιση του προγράμματος-πελάτη και διακομιστή. Εάν έχετε ήδη ολοκληρώσει μια Γνωρίστε και δημιουργήθηκε αυτών των πολιτικών, δεν χρειάζεται να το κάνετε ξανά. Επειδή ολοκληρώσει το Γνωρίστε, δεν θα πρέπει πρέπει να ρυθμίσετε νέες πολιτικές για walk-throughs προγράμματος-πελάτη στην τοποθεσία.

## <a name="add-configuration-to-your-serverjs-file"></a>Προσθήκη ρύθμισης παραμέτρων στο αρχείο σας server.js

Για να διαβάσετε τις τιμές από την `config.js` αρχείο, θα δημιουργηθεί, προσθέστε το `.config` αρχείου ως απαιτούμενη πόρο στην εφαρμογή σας και, στη συνέχεια, ορίστε τις καθολικές μεταβλητές με εκείνα του `config.js` εγγράφου.

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη:

`cd azuread`

Άνοιγμα του `server.js` αρχείο σε ένα πρόγραμμα επεξεργασίας. Προσθέστε τις ακόλουθες πληροφορίες:

```Javascript
var config = require('./config');
```
Προσθήκη νέας ενότητας για να `server.js` που περιλαμβάνει τον ακόλουθο κώδικα:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Στη συνέχεια, ας προσθέσουμε ορισμένα σύμβολα κράτησης θέσης για τους χρήστες που λαμβάνουμε από εφαρμογές μας κλήσης.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Ας προχωρήσουμε και δημιουργήστε πολύ μας καταγραφής.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Προσθέστε τις πληροφορίες MongoDB μοντέλο και σχήμα με τη χρήση Mongoose

Την προηγούμενη Παρασκευή αποφέρει όπως μπορείτε να αυτά τα τρία αρχεία μαζί σε μια υπηρεσία REST API.

Για Γνωρίστε αυτό, χρησιμοποιήστε MongoDB για να αποθηκεύετε τις εργασίες, όπως περιγράφεται παραπάνω.

Στο το `config.js` αρχείο, που ονομάζεται σας βάσης δεδομένων **tasklist**. Αυτό το όνομα ήταν ό, τι τοποθετείτε στο τέλος του `mongoose_auth_local` διεύθυνση URL σύνδεσης. Δεν χρειάζεται να δημιουργήσετε εκ των προτέρων αυτήν τη βάση δεδομένων στο MongoDB. Δημιουργεί τη βάση δεδομένων για εσάς κατά την πρώτη εκτέλεση της εφαρμογής του διακομιστή σας.

Αφού ενημερώνετε το διακομιστή της βάσης δεδομένων MongoDB για να χρησιμοποιήσετε, πρέπει να συντάξετε ορισμένες επιπλέον κώδικα για να δημιουργήσετε το μοντέλο και διάταξης για τις εργασίες του διακομιστή σας.

### <a name="expand-the-model"></a>Ανάπτυξη του μοντέλου

Αυτό το μοντέλο σχήματος είναι απλή. Μπορείτε να το αναπτύξετε όπως απαιτείται.

`owner`: Ποιος έχει ανατεθεί η εργασία. Αυτό το αντικείμενο είναι μια **συμβολοσειρά**.  

`Text`: Η ίδια η εργασία. Αυτό το αντικείμενο είναι μια **συμβολοσειρά**.

`date`: Η ημερομηνία κατά την οποία η εργασία είναι απαιτητή. Αυτό το αντικείμενο είναι ένα **ημερομηνίας/ώρας**.

`completed`: Εάν η εργασία έχει ολοκληρωθεί. Αυτό το αντικείμενο είναι μια **δυαδική**.

### <a name="create-the-schema-in-the-code"></a>Δημιουργία του σχήματος στον κώδικα

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη:

`cd azuread`

Άνοιγμα του `server.js` αρχείο σε ένα πρόγραμμα επεξεργασίας. Προσθέστε τις ακόλουθες πληροφορίες κάτω από την καταχώρηση της ρύθμισης παραμέτρων:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Μπορείτε να δημιουργήσετε πρώτα το σχήμα και, στη συνέχεια, μπορείτε να δημιουργήσετε ένα μοντέλο αντικειμένου που χρησιμοποιείτε για να αποθηκεύσετε τα δεδομένα σας σε όλο τον κωδικό, όταν ορίζετε τις **διαδρομές**.

## <a name="add-routes-for-your-rest-api-task-server"></a>Προσθέσετε διαδρομές για το διακομιστή εργασιών REST API

Τώρα που έχετε μοντέλου βάσης δεδομένων για να εργαστείτε με, προσθέστε τις διαδρομές που χρησιμοποιείτε για το διακομιστή REST API.

### <a name="about-routes-in-restify"></a>Σχετικά με διαδρομές στον Restify

Δρομολογεί λειτουργούν στο Restify με τον ίδιο τρόπο που λειτουργούν όταν χρησιμοποιούν στη στοίβα Express. Μπορείτε να καθορίσετε διαδρομές, χρησιμοποιώντας το URI που θεωρείτε ότι οι εφαρμογές προγράμματος-πελάτη για να καλέσετε. 

Ένα τυπικό μοτίβο για δρομολόγηση Restify είναι:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify και Express μπορούν να παρέχουν πολύ βαθύτερη λειτουργιών, όπως τον ορισμό τύπους εφαρμογών και κάνοντας σύνθετες δρομολόγηση σε διαφορετική τελικά σημεία. Για τους σκοπούς αυτής της εκμάθησης, θα σας να διατηρήσετε αυτές τις διαδρομές απλό.

#### <a name="add-default-routes-to-your-server"></a>Προσθέστε δρομολογεί προεπιλεγμένη στο διακομιστή

Τώρα μπορείτε να προσθέσετε τις βασικές διαδρομές CRUD για τη **Δημιουργία** και **λίστα** για μας REST API. Μπορείτε να βρείτε άλλες διαδρομές στο το `complete` κλάδο του δείγματος.

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη:

`cd azuread`

Άνοιγμα του `server.js` αρχείο σε ένα πρόγραμμα επεξεργασίας. Κάτω από τις καταχωρήσεις βάσης δεδομένων που κάνατε παραπάνω προσθέστε τις ακόλουθες πληροφορίες:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Προσθήκη χειρισμού σφαλμάτων για τις διαδρομές

Προσθήκη ορισμένες χειρισμού σφαλμάτων, έτσι ώστε να μπορείτε να επικοινωνείτε αντιμετωπίζετε προβλήματα με τον πελάτη με τον τρόπο που το να κατανοήσετε.

Προσθέστε τον ακόλουθο κώδικα:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Δημιουργία διακομιστή

Τώρα έχετε ορίσει τη βάση δεδομένων και τοποθετήστε τις διαδρομές στη θέση. Το τελευταίο πράγμα που πρέπει να κάνετε είναι να προσθέσετε την παρουσία διακομιστή που διαχειρίζεται τις κλήσεις σας.

Restify και Express παρέχει πολλά επίπεδα προσαρμογής για ένα διακομιστή REST API, αλλά χρησιμοποιήσουμε το πιο βασικής ρύθμισης εδώ. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Προσθέστε τις διαδρομές στο διακομιστή (χωρίς έλεγχο ταυτότητας)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Προσθέστε τον έλεγχο ταυτότητας στο διακομιστή REST API

Τώρα που έχετε εκτελείται διακομιστή REST API, μπορείτε να την κάνετε χρήσιμες έναντι Azure AD.

Από τη γραμμή εντολών, αλλάξτε τον κατάλογο για να `azuread`, εάν δεν υπάρχει ήδη:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Χρησιμοποιήστε το OIDCBearerStrategy που περιλαμβάνεται στο passport-azure ad


> [AZURE.TIP]
Όταν γράφετε APIs, θα πρέπει πάντα συνδέσετε τα δεδομένα σε κάποιο στοιχείο μοναδικά από τον κωδικό που ο χρήστης δεν μπορεί να πλαστογραφούν. Όταν ο διακομιστής αποθηκεύει ToDo στοιχείων, κάνει επομένως, με βάση το **αναγνωριστικό αντικειμένου** του χρήστη στο το διακριτικό (που ονομάζεται έως token.oid), θα μεταφερθείτε στο πεδίο "κάτοχος". Αυτή η τιμή εξασφαλίζει ότι μόνο αυτός ο χρήστης πρόσβαση τα δικά τους στοιχεία ToDo. Το API των "κάτοχος," είναι χωρίς έκθεσης, ώστε να έναν εξωτερικό χρήστη να ζητήσετε στοιχεία ToDo άλλων χρηστών, ακόμα και αν είναι με έλεγχο ταυτότητας.

Στη συνέχεια, χρησιμοποιήστε τη στρατηγική φορέα που παρέχεται με το `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport χρησιμοποιεί το ίδιο μοτίβο για όλες τις στρατηγικές. Μεταβίβαση αυτό μια `function()` που έχει `token` και `done` ως παράμετροι. Η στρατηγική διατίθεται ξανά σε εσάς αφού ολοκληρωθεί όλες τις εργασίες της. Μπορείτε, στη συνέχεια, θα πρέπει να αποθηκεύετε το χρήστη και να αποθηκεύετε το διακριτικό ώστε δεν χρειάζεται να ζητήσετε ξανά.

> [AZURE.IMPORTANT]
Ο κώδικας παραπάνω χρειάζονται όλοι οι χρήστες που θα συμβεί για τον έλεγχο ταυτότητας με το διακομιστή σας. Αυτή η διαδικασία είναι γνωστό ως autoregistration. Σε διακομιστές παραγωγής, δεν σας επιτρέπουν να σε οποιαδήποτε στους χρήστες πρόσβαση του API χωρίς πρώτα να τα ακολουθήσετε μια διαδικασία δήλωσης. Αυτή η διαδικασία είναι συνήθως το μοτίβο που βλέπετε στις εφαρμογές καταναλωτή που σας επιτρέπουν να καταχωρήσετε χρησιμοποιώντας το Facebook, αλλά, στη συνέχεια, ζητήστε να συμπληρώσετε πρόσθετες πληροφορίες. Εάν αυτό το πρόγραμμα δεν γραμμής εντολών προγράμματος, θα σας θα μπορούσε να έχει εξαχθεί το μήνυμα ηλεκτρονικού ταχυδρομείου από το διακριτικό αντικείμενο που επιστρέφεται και, στη συνέχεια, σας ζητηθεί στους χρήστες να συμπληρώνουν πρόσθετες πληροφορίες. Επειδή αυτό είναι ένα δείγμα, προσθέσουμε τους σε μια βάση δεδομένων στη μνήμη.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Εκτελέστε την εφαρμογή του διακομιστή για να επιβεβαιώσετε ότι το απορρίπτει

Μπορείτε να χρησιμοποιήσετε `curl` για να δείτε εάν έχετε τώρα OAuth2 προστασία σε σχέση με τα τελικά σημεία σας. Οι κεφαλίδες που επιστρέφονται πρέπει να είναι αρκετά να σας ενημερώσει ότι είστε στη σωστή διαδρομή.

Βεβαιωθείτε ότι η παρουσία σας MongoDB εκτελείται:

    $sudo mongodb

Μεταβείτε στον κατάλογο και να εκτελέσετε το διακομιστή:

    $ cd azuread
    $ node server.js

Σε ένα νέο παράθυρο τερματικού, εκτελέστε`curl`

Δοκιμάστε μια βασική ΔΗΜΟΣΊΕΥΣΗ:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Ένα σφάλμα 401 είναι την απάντηση που θέλετε. Υποδεικνύει ότι το επίπεδο Passport προσπαθεί να ανακατευθύνετε στο τελικό σημείο εξουσιοδότηση.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Τώρα έχετε μια υπηρεσία REST API που χρησιμοποιεί το OAuth2

Έχετε υλοποιήσει μια REST API με τη χρήση Restify και OAuth! Τώρα έχετε επαρκή κώδικα, έτσι ώστε να μπορείτε να συνεχίσετε να αναπτύξετε την υπηρεσία και να δημιουργήσετε σε αυτό το παράδειγμα. Που έχει γίνει όσον αφορά μπορείτε να κάνετε με αυτόν το διακομιστή χωρίς να χρησιμοποιείτε ένα πρόγραμμα-πελάτη συμβατό με OAuth2. Για αυτό το επόμενο βήμα, χρησιμοποιήστε μια επιπλέον Γνωρίστε όπως μας αναλυτικές οδηγίες για [σύνδεση σε μια τοποθεσία web API με χρήση του iOS με B2C](active-directory-b2c-devquickstarts-ios.md) .


## <a name="next-steps"></a>Επόμενα βήματα

Τώρα μπορείτε να μετακινήσετε σε θέματα για προχωρημένους, όπως:

[Σύνδεση σε μια τοποθεσία web API χρησιμοποιώντας iOS με B2C](active-directory-b2c-devquickstarts-ios.md)
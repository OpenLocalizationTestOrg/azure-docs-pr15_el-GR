<properties
    pageTitle="Azure AD NodeJS γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε ένα API Web REST Node.js που ενοποιείται με το Azure AD για έλεγχο ταυτότητας."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="getting-started-with-web-api-for-node"></a>Γρήγορα αποτελέσματα με το WEB API για κόμβου

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

**Passport** είναι το ενδιάμεσο ελέγχου ταυτότητας για Node.js. Εξαιρετικά ευέλικτη και λειτουργική, Passport μπορεί να γίνει unobtrusively απόθεση οποιονδήποτε βασίζεται Express ή Resitify εφαρμογής web. Ένα ολοκληρωμένο σύνολο στρατηγικές υποστηρίζει έλεγχο ταυτότητας χρησιμοποιώντας ένα όνομα χρήστη και τον κωδικό πρόσβασης, Facebook, Twitter και πολλά άλλα. Έχουμε αναπτύξει μια στρατηγική για το Microsoft Azure Active Directory. Θα εγκαταστήσετε αυτήν τη λειτουργική μονάδα και, στη συνέχεια, προσθέστε το Microsoft Azure Active Directory `passport-azure-ad` προσθήκης.

Για να το κάνετε αυτό, θα πρέπει να:

1. Καταχωρήστε μια εφαρμογή του Azure AD
2. Ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε Passport azure ad-passport προσθήκης.
3. Ρύθμιση παραμέτρων μιας εφαρμογής προγράμματος-πελάτη για να καλέσετε το για να κάνετε λίστα API Web

Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).

> [AZURE.NOTE] Σε αυτό το άρθρο δεν καλύπτει Διαχείριση προφίλ με Azure AD B2C και πώς μπορείτε να υλοποιήσετε εισόδου, εγγραφής.  Εστιάζει στην κλήση web APIs μετά το ήδη τον έλεγχο ταυτότητας χρήστη.  Εάν δεν το έχετε κάνει ήδη, πρέπει να ξεκινά με το [πώς μπορείτε να ενοποιήσετε το έγγραφο Azure Active Directory](./develop/active-directory-how-to-integrate.md) για να μάθετε περισσότερα σχετικά με τα βασικά στοιχεία του Azure Active Directory.


Θα σας έχετε κυκλοφορήσει όλων των τον πηγαίο κώδικα για αυτό εκτελείται το παράδειγμα στο GitHub κάτω από μια άδεια χρήσης του MIT, επομένως όποιους κλωνοποίηση (ή ακόμη καλύτερα, διακλάδωσης!) και παροχή σχολίων και ελκυστική αιτήσεις.

## <a name="about-nodejs-modules"></a>Σχετικά με τις λειτουργικές μονάδες Node.js

Θα χρησιμοποιήσουμε λειτουργικές μονάδες Node.js σε αυτές τις οδηγίες. Λειτουργικές μονάδες είναι φορτιζόμενους πακέτα JavaScript που σας παρέχει λειτουργικότητα συγκεκριμένα για την εφαρμογή σας. Εγκατάσταση συνήθως λειτουργικές μονάδες, χρησιμοποιώντας το εργαλείο γραμμής εντολών Node.js NPM στον κατάλογο εγκατάστασης του NPM, αλλά ορισμένες λειτουργικές μονάδες, όπως η λειτουργική μονάδα HTTP, περιλαμβάνονται το βασικό πακέτο Node.js.
Εγκατεστημένες λειτουργικές μονάδες αποθηκεύονται στον κατάλογο node_modules στη ρίζα της εγκατάστασης του καταλόγου σας Node.js. Κάθε λειτουργική μονάδα στον κατάλογο node_modules διατηρεί το δικό του καταλόγου node_modules που περιέχει οποιαδήποτε λειτουργικές μονάδες που εξαρτάται και κάθε λειτουργική μονάδα απαιτείται έχει έναν κατάλογο node_modules. Αυτή η επαναλαμβανόμενη δομή καταλόγου αντιπροσωπεύει την εξάρτηση αλυσίδα.

Αυτήν τη δομή αλυσίδα εξάρτηση έχει ως αποτέλεσμα ένα μεγαλύτερο αποτύπωμα εφαρμογή, αλλά εγγυάται ότι πληρούνται όλες τις εξαρτήσεις και ότι η έκδοση του οι λειτουργικές μονάδες που χρησιμοποιούνται στην ανάπτυξη θα χρησιμοποιείται επίσης στο παραγωγής. Αυτό κάνει πιο προβλέψιμων η συμπεριφορά της εφαρμογής παραγωγής και αποτρέπει την τήρηση ιστορικού εκδόσεων προβλήματα που μπορεί να επηρεάσει τους χρήστες.

## <a name="1-register-a-azure-ad-tenant"></a>1. καταχώρηση Azure AD μισθωτή

Για να χρησιμοποιήσετε αυτό το δείγμα θα χρειαστεί έναν μισθωτή Azure Active Directory. Εάν δεν είστε βέβαιοι ποια μισθωτή είναι ή πώς μπορείτε να αποκτήσετε ένα, ανατρέξτε στο θέμα [Τρόπος για να λάβετε μια Azure AD μισθωτή](active-directory-howto-tenant.md).

## <a name="2-create-an-application"></a>2. Δημιουργία μιας εφαρμογής

Τώρα, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογο, πράγμα που παρέχει Azure AD ορισμένες πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας.  Τόσο η εφαρμογή προγράμματος-πελάτη και web API θα αναπαρίσταται από ένα μεμονωμένο **Αναγνωριστικό εφαρμογής** σε αυτήν την περίπτωση, εφόσον αυτά περιλαμβάνουν μία λογική εφαρμογής.  Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-how-applications-are-added.md). Εάν δημιουργείτε μια γραμμή του Business εφαρμογή [μπορεί να είναι χρήσιμες αυτές τις πρόσθετες οδηγίες](active-directory-applications-guiding-developers-for-lob-applications.md).

Βεβαιωθείτε ότι:

- Είσοδος στην πύλη του Azure διαχείρισης.
- Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**.
- Επιλέξτε το μισθωτή όπου θέλετε να καταχωρήσετε την εφαρμογή.
- Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή Προσθήκη στο το σχέδιο κάτω.
- Ακολουθήστε τις οδηγίες και δημιουργήστε μια νέα **εφαρμογή Web ή/και WebAPI**.
    - Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    - Το **Σύμβολο στη διεύθυνση URL** είναι τη βασική διεύθυνση URL της εφαρμογής.  Το δείγμα κώδικα προεπιλογή είναι `https://localhost:8080`.
    - Το **URI Αναγνωριστικό εφαρμογής** είναι ένα μοναδικό αναγνωριστικό για την εφαρμογή σας.  Η σύμβαση είναι να χρησιμοποιήσετε `https://<tenant-domain>/<app-name>`, π.χ.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα ρύθμιση παραμέτρων.

- ΥΠΕΝΘΎΜΙΣΗ: δημιουργία μιας **Εφαρμογής μυστικό** για την εφαρμογή σας και αντιγραφή του προς τα κάτω.  Θα το χρειαστείτε λίγο.
- ΥΠΕΝΘΎΜΙΣΗ: Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας.  Θα πρέπει επίσης το λίγο.


## <a name="3-download-nodejs-for-your-platform"></a>3. Κάντε λήψη node.js για την πλατφόρμα σας
Για να χρησιμοποιήσετε με επιτυχία αυτό το δείγμα, πρέπει να έχετε μια εργασία εγκατάσταση του Node.js.

Εγκαταστήστε Node.js από [http://nodejs.org](http://nodejs.org).

## <a name="4-install-mongodb-on-to-your-platform"></a>4. εγκατάσταση MongoDB με την πλατφόρμα σας

Για να χρησιμοποιήσετε με επιτυχία αυτό το δείγμα, πρέπει να έχετε μια εργασία εγκατάσταση του MongoDB. Θα χρησιμοποιήσουμε MongoDB για να μας persistant REST API όλων των παρουσιών διακομιστή.

Εγκαταστήστε MongoDB από [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Αυτόν τον Οδηγό προϋποθέτει ότι χρησιμοποιείτε τα τελικά σημεία εγκατάστασης και server προεπιλογή για MongoDB, δηλαδή την ώρα της σύνταξης αυτού: mongodb://localhost


## <a name="5-install-the-restify-modules-in-to-your-web-api"></a>5. Εγκαταστήστε τις λειτουργικές μονάδες Restify στη για το API Web

Θα χρησιμοποιήσουμε Resitfy για να δημιουργήσετε μας REST API. Restify ένα ελάχιστο και ευέλικτη πλαίσιο εφαρμογή Node.js προέρχεται από Express που έχει ένα ισχυρό σύνολο δυνατοτήτων για τη δημιουργία APIs ΥΠΌΛΟΙΠΑ επάνω σε σύνδεση.

### <a name="install-restify"></a>Εγκατάσταση Restify

Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στον κατάλογο azuread. Εάν δεν υπάρχει στον κατάλογο **azuread** , δημιουργήστε τον.

`cd azuread - or- mkdir azuread; cd azuread`

Πληκτρολογήστε την ακόλουθη εντολή:

`npm install restify`

Αυτή η εντολή εγκαθιστά Restify.

#### <a name="did-you-get-an-error"></a>ΛΆΒΑΤΕ ΈΝΑ ΜΉΝΥΜΑ ΣΦΆΛΜΑΤΟΣ;

Όταν χρησιμοποιείτε npm σε ορισμένα λειτουργικά συστήματα, ενδέχεται να εμφανιστεί ένα σφάλμα σφάλματος: EPERM, chmod ' / χρήστης/τοπική/Ανακύκλωσης /..' και μια αίτηση για να δοκιμάσετε να το λογαριασμό με δικαιώματα διαχειριστή. Αν συμβεί αυτό, χρησιμοποιήστε την εντολή sudo για την εκτέλεση npm σε υψηλότερο επίπεδο δικαιωμάτων.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>ΛΆΒΑΤΕ ΈΝΑ ΣΦΆΛΜΑ ΣΧΕΤΙΚΆ ΜΕ ΤΙΣ DTRACE;

Μπορείτε να δείτε κάτι τέτοιο κατά την εγκατάσταση του Restify:

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


Restify παρέχει ένα ισχυρό μηχανισμό για την ανίχνευση ΥΠΌΛΟΙΠΑ κλήσεων με το DTrace. Ωστόσο, πολλά λειτουργικά συστήματα δεν έχουν DTrace διαθέσιμη. Μπορείτε να αγνοήσετε αυτά τα σφάλματα.


Το αποτέλεσμα αυτής της εντολής πρέπει να είναι παρόμοια με το εξής:


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


## <a name="6-install-passportjs-in-to-your-web-api"></a>6. εγκατάσταση Passport.js στο στην τοποθεσία Web API

[Passport](http://passportjs.org/) είναι το ενδιάμεσο ελέγχου ταυτότητας για Node.js. Εξαιρετικά ευέλικτη και λειτουργική, Passport μπορεί να γίνει unobtrusively απόθεση οποιονδήποτε βασίζεται Express ή Resitify εφαρμογής web. Ένα ολοκληρωμένο σύνολο στρατηγικές υποστηρίζει έλεγχο ταυτότητας χρησιμοποιώντας ένα όνομα χρήστη και τον κωδικό πρόσβασης, Facebook, Twitter και πολλά άλλα. Έχουμε αναπτύξει μια στρατηγική για Azure Active Directory. Θα εγκαταστήσετε αυτήν τη λειτουργική μονάδα και, στη συνέχεια, προσθέστε τη στρατηγική Azure Active Directory προσθήκης.

Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στον κατάλογο azuread.

Πληκτρολογήστε την παρακάτω εντολή για να εγκαταστήσετε το passport.js

`npm install passport`

Το αποτέλεσμα της εντολής πρέπει να είναι παρόμοια με το εξής:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="7-add-passport-azure-ad-to-your-web-api"></a>7. Προσθέστε Passport-Azure AD στην τοποθεσία Web API

Στη συνέχεια, θα προσθέσουμε τη στρατηγική διακριτικό, χρησιμοποιώντας passport-azuread, μιας οικογένειας στρατηγικές που συνδέουν Azure Active Directory με Passport. Θα χρησιμοποιήσουμε αυτήν τη στρατηγική για τα διακριτικά φορέα σε αυτό το δείγμα Rest API.

> [AZURE.NOTE] Παρόλο που OAuth2 παρέχει ένα πλαίσιο στο οποίο μπορεί να εκδοθεί γνωστού διακριτικού τύπου, μόνο ορισμένων τύπων διακριτικού έχουν που αποκτήθηκε πιο ευρείες χρήση. Για την προστασία των τελικά σημεία, που έχει ενεργοποιήσει για να τα διακριτικά φορέα. Διακριτικά φορέα είναι τα πιο ευρέως εκδοθεί τύπο διακριτικού στη OAuth2 και πολλές υλοποιήσεις λαμβάνεται ως δεδομένο ότι τα διακριτικά φορέα είναι ο μόνος τύπος διακριτικού εκδοθεί.

Από τη γραμμή εντολών, μεταβείτε σε καταλόγους στον κατάλογο azuread

Πληκτρολογήστε την παρακάτω εντολή για την εγκατάσταση της λειτουργικής μονάδας passport-azure ad Passport.js:

`npm install passport-azure-ad`

Το αποτέλεσμα της εντολής πρέπει να είναι παρόμοια με το εξής:

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



## <a name="8-add-mongodb-modules-to-your-web-api"></a>8. Προσθήκη λειτουργικές μονάδες MongoDB για το API Web

Θα χρησιμοποιήσουμε MongoDB ως μας αποθήκευσης δεδομένων για αυτόν το λόγο, πρέπει να εγκαταστήσετε και τα δύο το χρησιμοποιείται ευρέως προσθήκης για να διαχειριστείτε τα μοντέλα και οι διατάξεις που ονομάζεται Mongoose, καθώς και το πρόγραμμα οδήγησης βάσης δεδομένων για MongoDB, που ονομάζεται επίσης MongoDB.


* `npm install mongoose`

## <a name="9--install-additional-modules"></a>9. εγκατάσταση πρόσθετες λειτουργικές μονάδες

Στη συνέχεια, θα μπορούμε να εγκαταστήσουμε τις υπόλοιπες απαιτούμενες λειτουργικές μονάδες.


Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στο φάκελο **azuread** Εάν δεν είναι ήδη εκεί:

`cd azuread`


Εισαγάγετε τις παρακάτω εντολές για να εγκαταστήσετε τις ακόλουθες ενότητες στον κατάλογό σας node_modules:

* `npm install assert-plus`
* `npm install bunyan`
* `npm update`


## <a name="10-create-a-serverjs-with-your-dependencies"></a>10. Δημιουργήστε μια server.js με τις εξαρτήσεις

Το αρχείο server.js θα να σας δώσει το μεγαλύτερο μέρος των μας λειτουργίες για το διακομιστή Web API. Θα σας θα προσθέσετε περισσότερες μας κώδικα σε αυτό το αρχείο. Για σκοπούς παραγωγής που θα refactor τη λειτουργικότητα στο σε μικρότερες αρχεία, όπως διαφορετικές διαδρομές και ελεγκτές. Για την επίδειξη αυτό, θα χρησιμοποιήσουμε server.js για αυτήν τη λειτουργία.

Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στο φάκελο **azuread** Εάν δεν είναι ήδη εκεί:

`cd azuread`

Δημιουργία μιας `server.js` αρχείου στο μας αγαπημένες επεξεργασίας και προσθέστε τις ακόλουθες πληροφορίες:

```Javascript
    'use strict';

    /**
    * Module dependencies.
    */

    var fs = require('fs');
    var path = require('path');
    var util = require('util');
    var assert = require('assert-plus');
    var bunyan = require('bunyan');
    var getopt = require('posix-getopt');
    var mongoose = require('mongoose/');
    var restify = require('restify');
    var passport = require('passport');
  var BearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Αποθηκεύστε το αρχείο. Θα σας θα επιστρέψετε σύντομα.

## <a name="11-create-a-config-file-to-store-your-azure-ad-settings"></a>11:. Δημιουργήστε ένα αρχείο ρύθμισης παραμέτρων για να αποθηκεύσετε τις ρυθμίσεις σας Azure AD

Αυτό το αρχείο κώδικα μεταφέρει τη ρύθμιση παραμέτρων από την πύλη Azure Active Directory Passport.js. Δημιουργήσατε αυτές τις τιμές παραμέτρων όταν προσθέσατε το API Web με την πύλη στο πρώτο μέρος από την αναλυτική παρουσίαση. Θα σας θα εξηγούν τι μπορείτε να τοποθετήσετε τις τιμές αυτές τις παραμέτρους Αφού αντιγράψετε τον κώδικα.


Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στο φάκελο **azuread** Εάν δεν είναι ήδη εκεί:

`cd azuread`

Δημιουργία μιας `config.js` αρχείου στο μας αγαπημένες επεξεργασίας και προσθέστε τις ακόλουθες πληροφορίες:

```Javascript
 exports.creds = {
     mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
     clientID: 'your client ID',
     audience: 'your application URL',
    // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
  // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
     identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
     validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
     passReqToCallback: false,
     loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

 };


```
Αποθηκεύστε το αρχείο.

## <a name="12-add-configuration-to-your-serverjs-file"></a>12. Προσθήκη ρύθμισης παραμέτρων στο αρχείο σας server.js

Πρέπει να διαβάζετε αυτές τις τιμές από το αρχείο ρύθμισης παραμέτρων που μόλις δημιουργήσατε κατά μήκος μας εφαρμογής. Για να το κάνετε αυτό, θα σας απλώς να προσθέσετε το αρχείο .config ως απαιτούμενη πόρου σε μας εφαρμογή και, στη συνέχεια, ορίστε τις καθολικές μεταβλητές με εκείνα του εγγράφου config.js

Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στο φάκελο **azuread** Εάν δεν είναι ήδη εκεί:

`cd azuread`

Άνοιγμα του `server.js` αρχείου στο μας αγαπημένες επεξεργασίας και προσθέστε τις ακόλουθες πληροφορίες:

```Javascript
var config = require('./config');
```
Στη συνέχεια, προσθέστε μια νέα ενότητα για να `server.js` με τον ακόλουθο κώδικα:

```Javascript
var options = {
    // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback,
    loggingLevel: config.creds.loggingLevel

};

// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;

// Our logger
var log = bunyan.createLogger({
    name: 'Azure Active Directory Bearer Sample',
         streams: [
        {
            stream: process.stderr,
            level: "error",
            name: "error"
        },
        {
            stream: process.stdout,
            level: "warn",
            name: "console"
        }, ]
});

  // if logging level specified, switch to it.
  if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
```

Αποθηκεύστε το αρχείο.



## <a name="13-add-the-mongodb-model-and-schema-information-using-moongoose"></a>13. Προσθέστε το μοντέλο MongoDB και πληροφορίες σχήματος με χρήση Moongoose

Τώρα όλα αυτό προετοιμασίας πρόκειται να ξεκινήσετε πληρωμής όπως θα σας ανέμου αυτά τα τρία αρχεία μαζί σε μια υπηρεσία REST API.

Για αυτόν τον οδηγό θα χρησιμοποιήσουμε MongoDB για την αποθήκευση μας εργασιών, όπως περιγράφεται στο ***βήμα 4***.

Εάν μπορείτε να ανακαλέσετε από το `config.js` αρχείου που δημιουργήσαμε στο ***βήμα 11*** μας που ονομάζεται στη βάση δεδομένων μας `tasklist` με εκείνη που θα σας τοποθέτηση στο τέλος της η διεύθυνση URL μας mogoose_auth_local σύνδεσης. Δεν χρειάζεται να δημιουργήσετε εκ των προτέρων αυτήν τη βάση δεδομένων σε MongoDB, θα δημιουργήσει αυτό για μας στην πρώτη εκτέλεση μας εφαρμογής διακομιστή (αν υποθέσουμε ότι δεν υπάρχει ήδη).

Τώρα που θα σας έχετε σχετική οδηγία διακομιστή ποια MongoDB βάση δεδομένων που θα σας θα θέλατε να χρησιμοποιήσετε, πρέπει να γράψετε ορισμένες επιπλέον κώδικα για να δημιουργήσετε το μοντέλο και διάταξης για τις εργασίες του διακομιστή μας.

#### <a name="discussion-of-the-model"></a>Συζήτηση του μοντέλου

Το μοντέλο σχήματος είναι πολύ απλής και αναπτύσσετε όπως απαιτείται.

ΌΝΟΜΑ - το όνομα που έχει ανατεθεί η εργασία. Μια ***συμβολοσειρά***

Η ΕΡΓΑΣΊΑ - η ίδια η εργασία. Μια ***συμβολοσειρά***

ΗΜΕΡΟΜΗΝΊΑ - ημερομηνίας που είναι απαιτητή στην εργασία. ***Ημερομηνίας και ΏΡΑΣ***

ΟΛΟΚΛΉΡΩΣΗ - εάν η εργασία έχει ολοκληρωθεί ή όχι. Μια ***ΔΥΑΔΙΚΉ***

#### <a name="creating-the-schema-in-the-code"></a>Τη δημιουργία του σχήματος στον κώδικα


Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στο φάκελο **azuread** Εάν δεν είναι ήδη εκεί:

`cd azuread`

Άνοιγμα του `server.js` αρχείου στο μας αγαπημένες επεξεργασίας και προσθέστε τις ακόλουθες πληροφορίες κάτω από την καταχώρηση της ρύθμισης παραμέτρων:

```Javascript
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    task: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Όπως μπορείτε να καταλάβετε από τον κωδικό, θα σας δημιουργήσει το σχήμα και, στη συνέχεια, δημιουργήστε ένα μοντέλο αντικειμένου θα χρησιμοποιήσουμε να αποθηκεύει τα δεδομένα σε όλο τον κωδικό, όταν ορισμού μας ***δρομολογεί***.

## <a name="14-add-our-routes-for-our-task-rest-api-server"></a>14. προσθέσετε μας διαδρομές για το διακομιστή εργασιών REST API

Τώρα που έχουμε μοντέλου βάσης δεδομένων για να εργαστείτε, ας προσθέσουμε τις διαδρομές, θα χρησιμοποιήσουμε μας διακομιστής REST API.

### <a name="about-routes-in-restify"></a>Σχετικά με διαδρομές σε Restify

Δρομολογεί λειτουργούν στο Restify με τον ίδιο ακριβή τρόπο ό, τι χρησιμοποιώντας στη στοίβα Express. Μπορείτε να καθορίσετε δρομολογεί χρησιμοποιώντας το URI που θεωρείτε ότι οι εφαρμογές προγράμματος-πελάτη για να καλέσετε. Συνήθως, μπορείτε να καθορίσετε τις διαδρομές σε ένα ξεχωριστό αρχείο. Για τους σκοπούς μας, καταχωρήστε μας δρομολογεί στο αρχείο server.js. Συνιστάται να συντελεστής αυτά τα δικό τους αρχείο για χρήση παραγωγής.

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


Αυτό είναι το μοτίβο στο πιο βασικές επίπεδο. Resitfy (και Express) παρέχουν πολύ βαθύτερη functionaltiy όπως τον ορισμό τύπους εφαρμογών και κάνοντας σύνθετες δρομολόγηση σε διαφορετική τελικά σημεία. Για τους σκοπούς μας, θα σας θα διατηρήσει πολύ απλώς αυτές τις διαδρομές.

### <a name="1-add-default-routes-to-our-server"></a>1. Προσθήκη στο προεπιλεγμένο δρομολογεί μας διακομιστή

Θα σας τώρα θα προσθέσετε τις βασικές διαδρομές CRUD για δημιουργία, ανάκτηση, της ενημερωμένης έκδοσης και να διαγράψετε.

Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στο φάκελο **azuread** Εάν δεν είναι ήδη εκεί:

`cd azuread`

Άνοιγμα του `server.js` αρχείου στο μας αγαπημένες επεξεργασίας και προσθέστε τις ακόλουθες πληροφορίες κάτω από τις καταχωρήσεις βάσης δεδομένων που κάνατε παραπάνω:

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

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
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


// Delete a task by name

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

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

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initalize the database as stated in the README?");
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

### <a name="2-next-lets-add-some-error-handling-in-our-apis"></a>2. Επόμενο, ας προσθέσουμε ορισμένα χειρισμού στα μας API σφαλμάτων:

```

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


## <a name="15-create-your-server"></a>15. Δημιουργία διακομιστή σας!

Έχουμε μας βάσης δεδομένων που ορίζονται από το, έχουμε μας δρομολογεί στη θέση και το τελευταίο πράγμα που πρέπει να κάνετε είναι να προσθέσετε μας παρουσία διακομιστή που θα διαχειρίζεται μας κλήσεις.

Restify (και Express) έχουν πολλές βαθύ προσαρμογής, μπορείτε να κάνετε για ένα διακομιστή REST API, αλλά ξανά, θα χρησιμοποιήσουμε το πιο βασικής ρύθμισης για τους σκοπούς μας.

```Javascript
/**
 * Our Server
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
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
```

## <a name="16-adding-the-routes-to-the-server-without-authentication-for-now"></a>16. προσθέτοντας τις διαδρομές στο διακομιστή (χωρίς έλεγχο ταυτότητας προς το παρόν)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="17-before-we-add-oauth-support-lets-run-the-server"></a>17. πριν προσθέτουμε το διακριτικό υποστήριξης, ας εκτελέσετε στο διακομιστή.

Δοκιμάστε το διακομιστή πριν προσθέσουμε ελέγχου ταυτότητας

Ο ευκολότερος τρόπος για να το κάνετε αυτό είναι χρησιμοποιώντας καμπύλη σε μια γραμμή εντολών. Πριν το κάνουμε, χρειαζόμαστε ένα απλό βοηθητικό πρόγραμμα που σας επιτρέπει να ανάλυση εξόδου ως JSON. Για να το κάνετε, εγκαταστήστε το εργαλείο json όπως όλα τα παρακάτω παραδείγματα που χρησιμοποιούν.

`$npm install -g jsontool`

Αυτό εγκαθιστά το εργαλείο JSON καθολικά. Τώρα που έχετε θα σας να πραγματοποιηθεί που – ας δούμε με το διακομιστή:

Πρώτα, βεβαιωθείτε ότι εκτελείται το isntance monogoDB...

`$sudo mongod`

Στη συνέχεια, αλλάξτε τον κατάλογο και έναρξη curling...

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 200 OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Στη συνέχεια, να προσθέσουμε μια εργασία με αυτόν τον τρόπο:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

Η απόκριση πρέπει να είναι:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
Και παρατίθενται εργασίες για Brandon αυτόν τον τρόπο:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Εάν όλες οι αυτό λειτουργεί, συνεχίζουμε να είστε έτοιμοι να προσθέσετε διακριτικό στο διακομιστή REST API.

**Έχετε ένα διακομιστή REST API με MongoDB!**


## <a name="18-add-authentication-to-our-rest-api-server"></a>18. Προσθήκη ελέγχου ταυτότητας διακομιστή μας REST API

Τώρα που έχουμε μια ενσωματωμένη REST API (congrats, btw!), ας ξεκινήσουμε για να πραγματοποιήσετε χρήσιμες έναντι Azure AD.

Από τη γραμμή εντολών, αλλάξτε σε καταλόγους στο φάκελο **azuread** Εάν δεν είναι ήδη εκεί:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: Χρησιμοποιήστε την OIDCBearerStrategy που περιλαμβάνεται στο passport-azure ad

Μέχρι στιγμής θα σας έχει δημιουργηθεί ένα τυπικό διακομιστή TODO ΥΠΌΛΟΙΠΑ χωρίς κανένα είδος εξουσιοδότησης. Αυτό είναι πού θα σας ξεκινά με που μαζί.

Αρχικά, πρέπει να υποδείξετε ότι θέλουμε να χρησιμοποιήσετε Passport. Τοποθετήσετε αυτό το δικαίωμα μετά από τις άλλες ρυθμίσεις του διακομιστή:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Κατά τη σύνταξη APIs πρέπει πάντα συνδέσετε τα δεδομένα σε κάποιο στοιχείο μοναδικά από τον κωδικό που ο χρήστης δεν μπορεί να πλαστογραφούν. Όταν αυτός ο διακομιστής αποθηκεύει TODO στοιχεία, αποθηκεύει τις με βάση το Αναγνωριστικό αντικειμένου του χρήστη στο το διακριτικό (που ονομάζεται μέσω token.oid) που θέτει στο πεδίο "κάτοχος". Αυτό εξασφαλίζει ότι μόνο αυτός ο χρήστης να αποκτήσετε πρόσβαση προσωπική εκκρεμείς εργασίες και κανένας άλλος μπορούν να έχουν πρόσβαση το εκκρεμείς εργασίες που εισάγονται. Δεν υπάρχει καμία έκθεσης στη το API των "κάτοχος", ώστε να έναν εξωτερικό χρήστη να ζητήσετε εκκρεμείς εργασίες του άλλου, ακόμα και αν είναι με έλεγχο ταυτότητας.

Στη συνέχεια, ας χρησιμοποιήσουμε τη στρατηγική φορέα που παρέχεται με το passport-azure ad. Μόλις δείτε τον κώδικα προς το παρόν, θα επεξηγώ το λίγο. Τοποθέτηση αυτό αφού τι pated παραπάνω:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/

var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.sub === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var bearerStrategy = new BearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                users.push(token);
                owner = token.sub;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(bearerStrategy);
```

Passport χρησιμοποιεί ένα μοτίβο παρόμοια για όλους είναι στρατηγικές (Twitter, Facebook, κ.λπ.) που όλες οι συντάκτες στρατηγικής συμμορφώνεται με. Εξετάζοντας τη στρατηγική βλέπετε μας μεταβίβαση αυτό μια function() που έχει ένα διακριτικό και τέλος ως τις παραμέτρους. Η στρατηγική θα νόμιμα επιστρέψει μας όταν κάνει όλα το της εργασίας. Όταν κάνει θέλουμε να αποθηκεύετε το χρήστη και απόκρυψη το διακριτικό, ώστε να δεν θα πρέπει να ζητήσετε από το ξανά.

> [AZURE.IMPORTANT]
Ο κώδικας παραπάνω μεταφέρει οποιοσδήποτε χρήστης που συμβαίνει για τον έλεγχο ταυτότητας με το διακομιστή. Αυτό είναι γνωστό ως αυτόματης καταχώρησης. Σε διακομιστές παραγωγής δεν θα θέλετε να επιτρέψετε οποιοσδήποτε χωρίς πρώτα να τα ακολουθήσετε μια διαδικασία δήλωσης αποφασίσετε. Αυτό είναι συνήθως το μοτίβο που βλέπετε στις εφαρμογές καταναλωτή που σας επιτρέπει να καταχωρήσετε με το Facebook, αλλά, στη συνέχεια, ζητήστε να συμπληρώσετε πρόσθετες πληροφορίες. Εάν αυτό δεν προγράμματος γραμμή εντολών, θα σας μπορεί να έχουν μόλις που έχουν εξαχθεί το μήνυμα ηλεκτρονικού ταχυδρομείου από το διακριτικό αντικείμενο που επιστρέφεται και, στη συνέχεια, σας ζητηθεί να συμπληρώσετε πρόσθετες πληροφορίες. Επειδή πρόκειται για ένα διακομιστή δοκιμής απλώς προσθέσουμε τους στη βάση δεδομένων στη μνήμη.

### <a name="2-finally-protect-some-endpoints"></a>2. Τέλος, προστασία ορισμένα τελικά σημεία

Να προστατεύσετε τα τελικά σημεία, καθορίζοντας την `passport.authenticate()` κλήσεων με το πρωτόκολλο που θέλετε να χρησιμοποιήσετε.

Ας επεξεργαστούμε μας δρομολόγηση μας κώδικα διακομιστή για να κάνετε κάτι πιο ενδιαφέρουσα:

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19. Εκτελέστε ξανά την εφαρμογή του διακομιστή και βεβαιωθείτε ότι το απορρίπτει που

Ας χρησιμοποιήσουμε `curl` ξανά για να δείτε αν έχουμε τώρα OAuth2 προστασία σε σχέση με τα τελικά σημεία. Θα κάνουμε αυτό πριν από την runnning οποιαδήποτε από το πρόγραμμα-πελάτη SDK σε σχέση με αυτό το τελικό σημείο. Οι κεφαλίδες επιστρέφονται πρέπει να είναι αρκετά να μας πείτε Ζητούμε προς τα κάτω τη σωστή διαδρομή.

Πρώτα, βεβαιωθείτε ότι η παρουσία σας monogoDB εκτελείται:

  $sudo mongod

Στη συνέχεια, αλλάξτε τον κατάλογο και έναρξη curling...

  $ cd azuread $ κόμβο server.js

Δοκιμάστε μια βασική ΔΗΜΟΣΊΕΥΣΗ:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Μια 401 είναι η απάντηση που αναζητάτε εδώ, καθώς αυτό υποδεικνύει ότι το επίπεδο Passport προσπαθεί να ανακατευθύνετε στο τελικό σημείο εξουσιοδότηση, που είναι ακριβώς αυτό που θέλετε.

## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Συγχαρητήρια! Έχετε υπηρεσίας REST API χρησιμοποιώντας OAuth2!

Να έχετε κάποιο μέτρο που μπορείτε να κάνετε με αυτόν το διακομιστή χωρίς να χρησιμοποιείτε ένα πρόγραμμα-πελάτη συμβατό OAuth2. Θα πρέπει να ακολουθήσετε μια πρόσθετες οδηγίες.

Εάν απλώς να αναζητάτε πληροφορίες σχετικά με το πώς μπορείτε να υλοποιήσετε ένα REST API χρησιμοποιώντας Restify και το OAuth2, έχετε περισσότερες από αρκετό κώδικα για να διατηρήσετε ανάπτυξη της υπηρεσίας και μάθετε πώς να δημιουργείτε σε αυτό το παράδειγμα.

Εάν σας ενδιαφέρει στα επόμενα βήματα στο ταξίδι σας ADAL, εδώ θα βρείτε ορισμένες υποστηριζόμενες ADAL οι υπολογιστές-πελάτες σας συνιστούμε να για να συνεχίσετε να εργάζεστε:

Απλώς κλωνοποίηση προς τα κάτω για τον υπολογιστή σας για προγραμματιστές και ρύθμιση παραμέτρων όπως αναφέρεται στην την αναλυτική παρουσίαση.

[ADAL για iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL για Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]

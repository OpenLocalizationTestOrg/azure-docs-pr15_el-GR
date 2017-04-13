<properties
    pageTitle="Azure AD v2.0 NodeJS στο Web App | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή web JS κόμβου που πραγματοποιεί εταιρικό ή σχολικό λογαριασμοί και οι χρήστες με δύο τον προσωπικό λογαριασμό Microsoft."
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

# <a name="add-sign-in-to-a-nodejs-web-app"></a>Προσθήκη εισόδου σε ένα nodeJS Web App


> [AZURE.NOTE]
    Δεν όλα τα σενάρια Azure Active Directory και δυνατότητες που υποστηρίζονται από το τελικό σημείο v2.0.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).


Εδώ θα χρησιμοποιήσουμε Passport για να:

- Πραγματοποιήστε είσοδο στο χρήστη στην εφαρμογή χρησιμοποιώντας Azure AD και το τελικό σημείο v2.0.
- Εμφάνιση ορισμένες πληροφορίες σχετικά με το χρήστη.
- Πραγματοποιήστε είσοδο στο χρήστη από την εφαρμογή.

**Passport** είναι το ενδιάμεσο ελέγχου ταυτότητας για Node.js. Εξαιρετικά ευέλικτη και λειτουργική, Passport μπορεί να γίνει unobtrusively απόθεση οποιονδήποτε βασίζεται Express ή Resitify εφαρμογής web. Ένα ολοκληρωμένο σύνολο στρατηγικές υποστηρίζει έλεγχο ταυτότητας χρησιμοποιώντας ένα όνομα χρήστη και τον κωδικό πρόσβασης, Facebook, Twitter και πολλά άλλα. Έχουμε αναπτύξει μια στρατηγική για το Microsoft Azure Active Directory. Θα εγκαταστήσετε αυτήν τη λειτουργική μονάδα και, στη συνέχεια, προσθέστε το Microsoft Azure Active Directory `passport-azure-ad` προσθήκης.

## <a name="download"></a>Λήψη

Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής σκελετό ως ένα .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) ή κλωνοποίηση το σκελετό:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Παρέχονται στο τέλος αυτού του προγράμματος εκμάθησης καθώς και οι δυνατότητες της εφαρμογής.

## <a name="1-register-an-app"></a>1. καταχώρηση εφαρμογής
Δημιουργία νέας εφαρμογής στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε αυτά τα [λεπτομερή βήματα](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας, θα χρειαστεί το συντομότερο.
- Προσθέστε την πλατφόρμα για την εφαρμογή σας στο **Web** .
- Εισαγάγετε το σωστό **Ανακατεύθυνση URI**. Η ανακατεύθυνση URI υποδεικνύει στο Azure AD όπου θα πρέπει να κατευθύνεται απαντήσεων ελέγχου ταυτότητας - η προεπιλογή για αυτό το πρόγραμμα εκμάθησης είναι `http://localhost:3000/auth/openid/return`.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. Προσθέστε προ-requisities στον κατάλογό σας

Από τη γραμμή εντολών, αλλαγή σε καταλόγους στον ριζικό φάκελο Εάν δεν είναι ήδη εκεί και εκτελέστε τις ακόλουθες εντολές:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

- Επίσης, θα σας έχετε χρήση `passport-azure-ad` στο το σκελετό από τη Γρήγορη εκκίνηση.

- `npm install passport-azure-ad`


Αυτό θα εγκαταστήσει τις βιβλιοθήκες εξαρτώνται από αυτόν ad azure passport.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε τη στρατηγική passport-κόμβου-js
Εδώ, θα σας θα ρυθμίσετε τις παραμέτρους του Express ενδιάμεσου λογισμικού για να χρησιμοποιήσετε το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  Passport θα χρησιμοποιηθεί για την έκδοση αιτήσεις εισόδου και sign-out, Διαχείριση περιόδου λειτουργίας του χρήστη, και λήψη πληροφοριών σχετικά με το χρήστη, μεταξύ άλλων.

-   Για να ξεκινήσετε, ανοίξτε το `config.js` file στον ριζικό κατάλογο του έργου και πληκτρολογήστε τις τιμές παραμέτρων της εφαρμογής σας σε το `exports.creds` ενότητας.
    -   Η `clientID:` είναι το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας στην πύλη εγγραφής.
    -   Η `returnURL` είναι η **Ανακατεύθυνση URI** που καταχωρήσατε στην πύλη.
    - Η `clientSecret` είναι το μυστικό που δημιουργήσατε στην πύλη.

- Στη συνέχεια ανοίξτε `app.js` αρχείων στη ρίζα της το έργο και προσθέστε την κλήση follwing για να καλέσετε το `OIDCStrategy` στρατηγικής που παρέχεται με το`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Μετά από αυτό, χρησιμοποιήστε τη στρατηγική σας μόνο στα οποία γίνεται αναφορά για το χειρισμό μας προσκλήσεων login

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2)
//
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    skipUserProfile: config.creds.skipUserProfile
    scope: config.creds.scope
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport χρησιμοποιεί ένα μοτίβο παρόμοια για όλους είναι στρατηγικές (Twitter, Facebook, κ.λπ.) που όλες οι συντάκτες στρατηγικής συμμορφώνεται με. Εξετάζοντας τη στρατηγική βλέπετε μας μεταβίβαση αυτό μια function() που έχει ένα διακριτικό και τέλος ως τις παραμέτρους. Η στρατηγική θα νόμιμα επιστρέψει μας όταν κάνει όλα το της εργασίας. Όταν κάνει θέλουμε να αποθηκεύετε το χρήστη και απόκρυψη το διακριτικό, ώστε να δεν θα πρέπει να ζητήσετε από το ξανά.

> [AZURE.IMPORTANT]
Ο κώδικας παραπάνω μεταφέρει οποιοσδήποτε χρήστης που συμβαίνει για τον έλεγχο ταυτότητας με το διακομιστή. Αυτό είναι γνωστό ως αυτόματης καταχώρησης. Σε διακομιστές παραγωγής δεν θα θέλετε να επιτρέψετε οποιοσδήποτε χωρίς πρώτα να τα ακολουθήσετε μια διαδικασία δήλωσης αποφασίσετε. Αυτό είναι συνήθως το μοτίβο που βλέπετε στις εφαρμογές καταναλωτή που σας επιτρέπει να καταχωρήσετε με το Facebook, αλλά, στη συνέχεια, ζητήστε να συμπληρώσετε πρόσθετες πληροφορίες. Εάν αυτό δεν ένα δείγμα εφαρμογής, θα σας μπορεί να έχουν μόλις που έχουν εξαχθεί το μήνυμα ηλεκτρονικού ταχυδρομείου από το διακριτικό αντικείμενο που επιστρέφεται και, στη συνέχεια, σας ζητηθεί να συμπληρώσετε πρόσθετες πληροφορίες. Επειδή πρόκειται για ένα διακομιστή δοκιμής απλώς προσθέσουμε τους στη βάση δεδομένων στη μνήμη.

- Στη συνέχεια, προσθέστε τις μεθόδους που θα σας επιτρέψει μας για να παρακολουθείτε συνδεδεμένου σε χρήστες, όπως απαιτείται από το Passport. Αυτό περιλαμβάνει σειριοποίησης και αποσειριοποίησης τις πληροφορίες του χρήστη:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

- Στη συνέχεια, προσθέστε τον κωδικό για να φορτώσετε το μηχανισμό express. Εδώ μπορείτε να δείτε χρησιμοποιήσουμε το προεπιλεγμένο /views και παρέχει μοτίβο /routes που Express.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Τέλος, ας προσθέσουμε τις διαδρομές ΔΗΜΟΣΊΕΥΣΗ που θα παραδώσετε τις αιτήσεις πραγματική σύνδεση προς το `passport-azure-ad` μηχανισμός:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authenitcation was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. χρήση Passport για την έκδοση αιτήσεις εισόδου και sign-out να Azure AD

Εφαρμογή σας είναι τώρα έχει ρυθμιστεί σωστά για να επικοινωνήσετε με το τελικό σημείο v2.0 χρησιμοποιεί το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  `passport-azure-ad`έχει ληφθεί φροντίσει όλες τις λεπτομέρειες καλή δημιουργία μηνύματα ελέγχου ταυτότητας, επικύρωση διακριτικά από Azure AD και τη διατήρηση περίοδο λειτουργίας χρήστη.  Όλες τις δυνατότητες που παραμένει είναι να δώσετε τους χρήστες σας ένας τρόπος για να εισέλθετε, πραγματοποιήστε έξοδο και συγκέντρωση πρόσθετες πληροφορίες στην ο συνδεδεμένος χρήστης.

- Πρώτα, σας επιτρέπει να προσθέσετε την προεπιλεγμένη, σύνδεσης, το λογαριασμό και αποσύνδεσης μεθόδους για να μας `app.js` αρχείο:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Ας, διαβάστε αυτές τις λεπτομέρειες:
    -   Το `/` δρομολόγηση θα σας ανακατευθύνει στην προβολή index.ejs διέλευση του χρήστη στην πρόσκληση σε (εάν υπάρχει)
    - Το `/account` δρομολόγηση θα πρώτα ***Βεβαιωθείτε ότι σας έχουν ελεγχθεί η ταυτότητά*** (μας υλοποίηση που κάτω από το στοιχείο) και, στη συνέχεια, ο χρήστης στο στοιχείο την αίτηση ώστε να μπορούμε να λαμβάνουμε πρόσθετες πληροφορίες σχετικά με το χρήστη.
    - Το `/login` δρομολόγηση θα καλούν μας azuread openidconnect ο έλεγχος ταυτότητας από `passport-azuread` και αν που δεν ολοκληρωθεί με επιτυχία θα ανακατευθύνετε το χρήστη ξανά /login
    - Το `/logout` θα απλώς κλήση του logout.ejs (και δρομολόγηση) που καταργεί τα cookies και, στη συνέχεια, επιστρέψετε στο χρήστη index.ejs


- Για το τελευταίο τμήμα της `app.js`, ας προσθέσουμε τη μέθοδο EnsureAuthenticated που χρησιμοποιείται σε `/account` παραπάνω.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

- Τέλος, ας στην πραγματικότητα δημιουργία ίδιο το διακομιστή στο `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. Δημιουργία των προβολών και δρομολογεί στο express για να εμφανίσετε το χρήστη στην τοποθεσία Web

Έχουμε μας `app.js` ολοκλήρωσης. Τώρα πρέπει απλώς να προσθέσετε την δρομολογεί και προβολές που θα εμφανίζονται οι πληροφορίες λαμβάνουμε για το χρήστη, καθώς και λαβή το `/logout` και `/login` δρομολογεί έχουμε δημιουργήσει.

- Δημιουργία του `/routes/index.js` δρομολόγηση κάτω από το ριζικό κατάλογο.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Δημιουργία του `/routes/user.js` δρομολόγηση κάτω από το ριζικό κατάλογο

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Αυτές οι απλές διαδρομές θα απλώς περνούν από την αίτηση για να μας προβολές, συμπεριλαμβανομένου του χρήστη, εάν υπάρχει.

- Δημιουργία του `/views/index.ejs` προβολή κάτω από το ριζικό κατάλογο. Αυτή είναι μια απλή σελίδα που θα καλέσετε μας μεθόδους σύνδεσης και αποσύνδεσης και επιτρέψετε μας για να καταγράψετε τις πληροφορίες του λογαριασμού. Ειδοποίηση ότι μπορούμε να χρησιμοποιήσουμε τη μορφοποίηση υπό όρους `if (!user)` ως ο χρήστης που διαβιβάζεται μέσω της αίτησης είναι αποδείξεις έχουμε έναν συνδεδεμένο στο χρήστη.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Δημιουργία του `/views/account.ejs` προβολή κάτω από το ριζικό κατάλογο, έτσι ώστε να μπορούμε να προβάλουμε πρόσθετες πληροφορίες που `passport-azuread` έχουν θέσει σε της αίτησης χρήστη.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Τέλος, ας κάνουμε αυτήν την εμφάνιση αρκετά, προσθέτοντας μια διάταξη. Δημιουργήστε το ' / Προβολή των views/layout.ejs κάτω από το ριζικό κατάλογο

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> |
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> |
            <a href="/account">Account</a> |
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Τέλος, δημιουργία και εκτέλεση της εφαρμογής σας!

Εκτέλεση `node app.js` και μεταβείτε στις επιλογές`http://localhost:3000`


Συνδεθείτε με έναν προσωπικό λογαριασμό Microsoft ή έναν εταιρικό ή σχολικό λογαριασμό και Παρατηρήστε πώς την ταυτότητα του χρήστη ενημερώνεται στη λίστα /account.  Τώρα έχετε μια εφαρμογή web ασφαλές χρησιμοποιώντας κλάδο τυπική πρωτόκολλα που μπορούν να ελέγχουν την ταυτότητα χρηστών με τις προσωπικές και εταιρικό/σχολικό τους λογαριασμούς.

##<a name="next-steps"></a>Επόμενα βήματα

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) [παρέχεται ως ένα .zip εδώ](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip), ή μπορείτε να αντιγράψει το το GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Τώρα μπορείτε να μετακινήσετε σε θέματα για προχωρημένους.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς μια node.js στο web χρησιμοποιώντας το τελικό σημείο v2.0 api >>](active-directory-v2-devquickstarts-node-api.md)

Για επιπλέον πόρους, ανατρέξτε στο θέμα:
- [Στον οδηγό για προγραμματιστές v2.0 >>](active-directory-appmodel-v2-overview.md)
- [Ετικέτα StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία [αυτήν τη σελίδα](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.

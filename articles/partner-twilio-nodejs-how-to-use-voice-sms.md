<properties 
    pageTitle="Χρήση Twilio για φωνής, VoIP και μηνυμάτων SMS στο Azure" 
    description="Μάθετε πώς να πραγματοποιήσω μια τηλεφωνική κλήση και να στείλετε ένα μήνυμα SMS με την υπηρεσία Twilio API σε Azure. Δείγματα κώδικα γραμμένο σε Node.js." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Χρήση Twilio για φωνής, VoIP και μηνυμάτων SMS στο Azure

Αυτός ο οδηγός παρουσιάζει πώς μπορείτε να δημιουργήσετε εφαρμογές που επικοινωνία με Twilio και node.js στην Azure.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Τι είναι το Twilio;

Twilio είναι μια πλατφόρμα API που σας διευκολύνει τους προγραμματιστές να πραγματοποίηση και λήψη τηλεφωνικών κλήσεων, αποστολή και λήψη μηνυμάτων κειμένου και ενσωμάτωση κλήσεις VoIP σε βασίζονται σε πρόγραμμα περιήγησης και εγγενούς εφαρμογές για κινητές συσκευές.  Ας σύντομα δούμε πώς λειτουργεί αυτό πριν καταδύσεις.

### <a name="receiving-calls-and-text-messages"></a>Λαμβάνει κλήσεις και μηνύματα κειμένου

Twilio επιτρέπει στους προγραμματιστές να [αγοράσετε αριθμών τηλεφώνου προγραμματιζόμενο] [ purchase_phone] που μπορούν να χρησιμοποιηθούν για να στείλετε και να λαμβάνουν κλήσεις και μηνύματα κειμένου.  Όταν ένας αριθμός Twilio λαμβάνει μια εισερχόμενη κλήση ή κείμενο, Twilio θα σας στέλνει την εφαρμογή web μια ΔΗΜΟΣΊΕΥΣΗ HTTP ή αίτημα GET, που σας ρωτά για οδηγίες σχετικά με τον τρόπο χειρισμού την κλήση ή κειμένου.  Ο διακομιστής θα ανταποκριθεί σε μια αίτηση HTTP του Twilio με [TwiML][twiml], ένα απλό σύνολο ετικετών XML που περιέχει οδηγίες σχετικά με τον τρόπο χειρισμού των μια κλήση ή κειμένου.  Θα σας θα δείτε παραδείγματα TwiML σε απλώς λίγο.

### <a name="making-calls-and-sending-text-messages"></a>Πραγματοποίηση κλήσεων και την αποστολή μηνυμάτων κειμένου

Καθιστώντας αιτήσεις HTTP για το API της υπηρεσίας web Twilio, τους προγραμματιστές να στείλετε μηνύματα κειμένου ή να προετοιμασία εξερχομένων τηλεφωνικές κλήσεις.  Για εξερχόμενες κλήσεις, ο προγραμματιστής πρέπει επίσης να καθορίσετε μια διεύθυνση URL που επιστρέφει TwiML οδηγίες για τον τρόπο χειρισμού την εξερχόμενη κλήση όταν είναι συνδεδεμένη.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Ενσωμάτωση VoIP δυνατότητες σε κώδικα περιβάλλοντος εργασίας Χρήστη (JavaScript, iOS ή Android)

Twilio παρέχει μια πλευρά του προγράμματος-πελάτη SDK, το οποίο μπορεί να μετατρέψετε οποιοδήποτε πρόγραμμα περιήγησης web επιφάνειας εργασίας, εφαρμογή iOS ή Android εφαρμογής σε ένα τηλέφωνο VoIP.  Σε αυτό το άρθρο θα σας εστιάζει σχετικά με τη χρήση VoIP κλήση στο πρόγραμμα περιήγησης.  Εκτός από το SDK JavaScript Twilio εκτελείται στο πρόγραμμα περιήγησης, μια εφαρμογή διακομιστή (μας εφαρμογή node.js) πρέπει να χρησιμοποιείται για την έκδοση ενός διακριτικού"τη δυνατότητα" στο πρόγραμμα-πελάτη JavaScript.  Μπορείτε να διαβάσετε περισσότερα σχετικά με τη χρήση VoIP με node.js [στο ιστολόγιο του αποκλίσεις Twilio][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Εγγραφή για Twilio (Microsoft προεξόφληση)

Πριν να χρησιμοποιήσετε τις υπηρεσίες Twilio, πρέπει να [εγγραφείτε για ένα λογαριασμό]πρώτη[signup].  Οι πελάτες Microsoft Azure λαμβάνουν μια ειδική έκπτωση - [φροντίστε να εγγραφούν εδώ][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Δημιουργία και ανάπτυξη μιας τοποθεσίας Web node.js Azure

Στη συνέχεια, θα πρέπει να δημιουργήσετε μια τοποθεσία Web node.js εκτελείται σε Azure.  [Το επίσημο τεκμηρίωση για αυτόν τον τρόπο βρίσκεται εδώ][azure_new_site].  Σε υψηλό επίπεδο, που θα ως εξής:

* Η εγγραφή στο λογαριασμό του Azure, εάν δεν έχετε ήδη
* Χρησιμοποιώντας την Κονσόλα διαχείρισης Azure για να δημιουργήσετε μια νέα τοποθεσία Web
* Προσθήκη προέλευσης στοιχείου ελέγχου υποστήριξης (θα υποθέσουμε ότι χρησιμοποιήσατε git)
* Δημιουργία αρχείου `server.js` με μια απλή node.js εφαρμογή web
* Για την ανάπτυξη αυτής της εφαρμογής απλό Azure

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Ρύθμιση παραμέτρων της λειτουργικής μονάδας Twilio

Στη συνέχεια, θα σας θα ξεκινήσει να συντάξετε μια απλή node.js εφαρμογή που χρησιμοποιεί το API Twilio.  Πριν ξεκινήσετε θα σας, πρέπει να ρυθμίσετε τις παραμέτρους μας Twilio διαπιστευτήρια λογαριασμού.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Ρύθμιση παραμέτρων διαπιστευτηρίων Twilio μεταβλητές περιβάλλοντος συστήματος

Για να κάνετε έλεγχο ταυτότητας προσκλήσεις σε σχέση με Twilio παρασκηνίου, χρειαζόμαστε λογαριασμό αναγνωριστικό ΑΣΦΑΛΕΊΑΣ και το διακριτικό auth, που λειτουργούν με το όνομα χρήστη και τον κωδικό πρόσβασης για το λογαριασμό Twilio μας. Είναι ο πιο ασφαλής τρόπος για να ρυθμίσετε τις παραμέτρους αυτές για χρήση με τη λειτουργική μονάδα κόμβου στο Azure μέσω του συστήματος περιβάλλον μεταβλητές, τις οποίες μπορείτε να ορίσετε απευθείας από την Κονσόλα διαχείρισης Azure.

Επιλέξτε την τοποθεσία Web node.js και κάντε κλικ στη σύνδεση "Ρύθμιση ΠΑΡΑΜΈΤΡΩΝ".  Εάν κάνετε κύλιση προς τα κάτω λίγο, θα δείτε μια περιοχή όπου μπορείτε να ορίσετε ιδιότητες ρύθμισης παραμέτρων για την εφαρμογή σας.  Εισαγάγετε τα διαπιστευτήριά σας λογαριασμό Twilio ([βρέθηκε στον πίνακα εργαλείων σας Twilio][twilio_dashboard]) όπως φαίνεται - φροντίστε να του δώσετε ένα όνομα "TWILIO_ACCOUNT_SID" και "TWILIO_AUTH_TOKEN", αντίστοιχα:

![Κονσόλα διαχείρισης Azure][azure-admin-console]

Αφού έχετε ρυθμίσει αυτές τις μεταβλητές, επανεκκινήστε την εφαρμογή σας στην κονσόλα Azure.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Δήλωση της λειτουργικής μονάδας Twilio στο package.json

Στη συνέχεια, πρέπει να δημιουργήσετε ένα package.json για τη Διαχείριση μας κόμβο εξαρτήσεις λειτουργική μονάδα μέσω [npm].  Στο ίδιο επίπεδο με το αρχείο "server.js" που δημιουργήσατε στην εκμάθηση Azure/node.js, δημιουργήστε ένα αρχείο με το όνομα "package.json".  Μέσα σε αυτό το αρχείο, τοποθετήστε τα εξής:

  {"όνομα": "όνομα εφαρμογής", "έκδοση": "0.0.1", "ιδιωτικό": την τιμή true, "δέσμες ενεργειών": {"εκκίνηση": "κόμβο διακομιστή"}, "εξαρτήσεις": {"Σύντομη": "3.1.0", "ejs": "*", "twilio": "*"}}

Αυτό δηλώνει τη λειτουργική μονάδα twilio ως μια εξάρτηση, καθώς και τα δημοφιλή [framework express web] [ express] και ο μηχανισμός πρότυπο EJS.  Εντάξει, τώρα θα σας είστε έτοιμοι - ας ορισμένες κώδικας σύνταξης!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Πραγματοποίηση κλήσης εξερχομένων

Ας δημιουργήσουμε μια απλή φόρμα που θα πραγματοποιήσετε μια κλήση σε έναν αριθμό που θα σας επιλέξτε.  Άνοιγμα του server.js και πληκτρολογήστε τον παρακάτω κώδικα.  Σημείωση όπου εμφανίζεται η ένδειξη "CHANGE_ME" - Τοποθέτηση το όνομα της τοποθεσίας Web azure εκεί:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Στη συνέχεια, δημιουργήστε έναν κατάλογο ονομάζονται "προβολές" - μέσα σε αυτόν τον κατάλογο, δημιουργήστε ένα αρχείο με το όνομα "index.ejs" με το εξής περιεχόμενο:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Τώρα, αναπτύξτε την τοποθεσία Web στον Azure και ανοίξτε το σπίτι σας.  Θα πρέπει να μπορείτε να εισαγάγετε τον αριθμό τηλεφώνου στο πεδίο κειμένου και να λάβετε μια κλήση από τον αριθμό Twilio!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Αποστολή μηνύματος SMS

Τώρα, ας ορίσουμε ένα περιβάλλον εργασίας χρήστη και χειρισμός λογικής φόρμας για να στείλετε ένα μήνυμα κειμένου.  Άνοιγμα του "server.js" και προσθέστε τον ακόλουθο κώδικα μετά την τελευταία κλήση "app.post":

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

Στο "views/index.ejs", προσθέστε μια άλλη φόρμα κάτω από το πρώτο για να υποβάλετε έναν αριθμό και ένα μήνυμα κειμένου:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Αναπτύξτε ξανά την εφαρμογή με το Azure και τώρα θα πρέπει να μπορείτε να υποβάλετε που σχηματίζουν και να στείλετε ένα μήνυμα κειμένου στον εαυτό σας (ή οποιονδήποτε από τους φίλους σας πλησιέστερη)!

<a id="nextsteps"/>
## <a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μάθει τα βασικά στοιχεία της χρήσης node.js και Twilio για να δημιουργήσετε εφαρμογές που επικοινωνία.  Αλλά αυτά τα παραδείγματα μόλις εκτός Προχείρου στην επιφάνεια της τι είναι δυνατή με Twilio και node.js.  Για χρήση Twilio με node.js περισσότερες πληροφορίες, ανατρέξτε στους παρακάτω πόρους:

* [Επίσημη λειτουργική μονάδα έγγραφα][docs]
* [Πρόγραμμα εκμάθησης για το VoIP με εφαρμογές node.js][voipnode]
* [Votr - ένα σε πραγματικό χρόνο SMS εκλογής εφαρμογή με node.js και CouchDB (τρία τμήματα)][votr]
* [Προγραμματισμός ζεύγος στο πρόγραμμα περιήγησης με node.js][pair]

Θα σας ελπίζετε θα χαρούμε πολύ παραβίασης ασφαλείας node.js και Twilio στην Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png




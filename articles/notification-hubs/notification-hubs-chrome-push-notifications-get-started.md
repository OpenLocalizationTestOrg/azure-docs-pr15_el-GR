<properties
    pageTitle="Αποστολή ειδοποιήσεων push σε εφαρμογές Chrome με διανομείς ειδοποίηση Azure | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια εφαρμογή Chrome."
    services="notification-hubs"
    keywords="ειδοποιήσεις push κινητές συσκευές, τις ειδοποιήσεις push, push ειδοποίηση, του chrome ειδοποιήσεων push"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Αποστολή ειδοποιήσεων push σε εφαρμογές Chrome με διανομείς ειδοποίηση Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push για μια εφαρμογή Chrome, που θα εμφανίζονται στο περιβάλλον του Google Chrome προγράμματος περιήγησης. Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσουμε μια εφαρμογή Chrome που λαμβάνει τις ειδοποιήσεις push με χρήση του [Google Cloud ανταλλαγής μηνυμάτων (GCM)](https://developers.google.com/cloud-messaging/). 

>[AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Το πρόγραμμα εκμάθησης σάς καθοδηγεί σε αυτά τα βασικά βήματα για να ενεργοποιήσετε τις ειδοποιήσεις push:

* [Ενεργοποίηση Google Cloud ανταλλαγή μηνυμάτων](#register)
* [Ρύθμιση των παραμέτρων σας διανομέα ειδοποίησης](#configure-hub)
* [Σύνδεση της εφαρμογής σας Chrome με την ενότητα "Ειδοποίηση"](#connect-app)
* [Στείλτε μια ειδοποίηση push για την εφαρμογή του Chrome](#send)
* [Πρόσθετες λειτουργίες και δυνατότητες](#next-steps)

>[AZURE.NOTE] Ειδοποιήσεις push της εφαρμογής Chrome δεν είναι γενικής χρήσης ειδοποιήσεις στο πρόγραμμα περιήγησης - αυτές είναι διαφορετικές για την επεκτασιμότητα του προγράμματος περιήγησης του μοντέλου (ανατρέξτε στο θέμα [Επισκόπηση εφαρμογές Chrome] για λεπτομέρειες). Εκτός από το πρόγραμμα περιήγησης υπολογιστή, Chrome εφαρμογές εκτελέσετε στο κινητό (iOS και Android) μέσω Apache Cordova. Ανατρέξτε στο θέμα [Chrome εφαρμογών στο κινητό] για να μάθετε περισσότερα.

Ρύθμιση παραμέτρων GCM και διανομείς ειδοποίηση Azure είναι ίδια με τη ρύθμιση των παραμέτρων για Android, επειδή το [Google Cloud ανταλλαγής μηνυμάτων για το Chrome] έχει καταργηθεί και το ίδιο GCM υποστηρίζει πλέον συσκευές Android και παρουσίες Chrome.

##<a id="register"></a>Ενεργοποίηση Google Cloud ανταλλαγή μηνυμάτων

1. Μεταβείτε στην τοποθεσία Web [Κονσόλα Google Cloud] , συνδεθείτε με τα διαπιστευτήριά σας Google λογαριασμού και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημιουργία έργου** . Πληκτρολογήστε ένα κατάλληλο **Όνομα έργου**και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημιουργία** .

    ![Google Cloud Console - Δημιουργία έργου][1]

2. Σημειώστε τον **Αριθμό έργου** στη σελίδα " **έργα** " για το έργο που μόλις δημιουργήσατε. Θα χρησιμοποιήσετε με το **Αναγνωριστικό αποστολέα GCM** στην εφαρμογή Chrome για την καταχώρηση με GCM.

    ![Κονσόλα Google Cloud - αριθμός έργου][2]

3. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **APIs & auth**, και, στη συνέχεια, κάντε κύλιση προς τα κάτω και επιλέξτε το στοιχείο εναλλαγής για να ενεργοποιήσετε την **Google Cloud ανταλλαγής μηνυμάτων για Android**. Δεν χρειάζεται να ενεργοποιήσετε **Google Cloud ανταλλαγής μηνυμάτων για το Chrome**.

    ![Κονσόλα Google Cloud - κλειδί διακομιστή][3]

4. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **διαπιστευτήρια** > **Δημιουργία νέου αριθμού-κλειδιού** > **Κλειδί διακομιστή** > **Δημιουργία**.

    ![Κονσόλα Google Cloud - διαπιστευτήρια][4]

5. Σημειώστε του **Αριθμού-κλειδιού API**του διακομιστή. Θα ρυθμίζετε αυτή από την ενότητα ειδοποίησης στη συνέχεια, για να το ενεργοποιήσετε για την αποστολή ειδοποιήσεων push σε GCM.

    ![Κονσόλα Google Cloud - API του αριθμού-κλειδιού][5]

##<a id="configure-hub"></a>Ρύθμιση των παραμέτρων σας διανομέα ειδοποίησης

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. στο το blade **Ρυθμίσεις** , επιλέξτε **Υπηρεσίες ειδοποιήσεων** και, στη συνέχεια, **Google (GCM)**. Εισαγάγετε τον αριθμό-κλειδί API και αποθήκευση.

&emsp;&emsp;![Ειδοποίηση Azure διανομείς - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Σύνδεση της εφαρμογής σας Chrome με την ενότητα "Ειδοποίηση"

Το Κέντρο ειδοποίηση σας τώρα έχει ρυθμιστεί ώστε να λειτουργεί με GCM και έχετε τις συμβολοσειρές σύνδεσης για να καταχωρήσετε την εφαρμογή σας να λαμβάνει και να στείλετε τις ειδοποιήσεις προώθησης. LK

###<a name="create-a-new-chrome-app"></a>Δημιουργία νέας εφαρμογής Chrome

Το παρακάτω δείγμα βασίζεται σε [Δείγμα GCM εφαρμογή Chrome] και χρησιμοποιεί τον προτεινόμενο τρόπο για να δημιουργήσετε μια εφαρμογή Chrome. Θα σας θα επισημάνετε τα βήματα συγκεκριμένα που σχετίζονται με Azure ειδοποίηση διανομείς. 

>[AZURE.NOTE] Συνιστάται να κάνετε λήψη του αρχείου προέλευσης για αυτήν την εφαρμογή Chrome από το [Δείγμα διανομέα ειδοποίηση εφαρμογής Chrome].

Η εφαρμογή Chrome δημιουργείται μέσω JavaScript και μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις συντάκτες προτιμώμενη word για τη δημιουργία του. Ακολουθεί πώς θα μοιάζει αυτής της εφαρμογής Chrome.

![Εφαρμογή Google Chrome][15]

1. Δημιουργήστε ένα φάκελο και ονομάστε το `ChromePushApp`. Φυσικά, το όνομα είναι αυθαίρετο - εάν ονομάσετε το κάτι διαφορετικό, βεβαιωθείτε ότι μπορείτε να αντικαταστήσετε τη διαδρομή στα τμήματα της απαιτείται κωδικός.

2. Κάντε λήψη του [crypto js βιβλιοθήκης] στο φάκελο που δημιουργήσατε κατά το δεύτερο βήμα. Αυτός ο φάκελος βιβλιοθήκη θα περιέχει δύο υποφακέλους: `components` και `rollups`.

3. Δημιουργία μιας `manifest.json` αρχείου. Όλες οι εφαρμογές Chrome δημιουργούνται αντίγραφα από ένα αρχείο δηλώσεων που περιέχει τα μετα-δεδομένα εφαρμογής και, πιο σημαντικό είναι ότι όλα τα δικαιώματα που έχουν εκχωρηθεί στην εφαρμογή, όταν ο χρήστης εγκαθιστά το.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Ειδοποίηση του `permissions` στοιχείου, το οποίο καθορίζει ότι αυτή η εφαρμογή Chrome θα μπορούν να λαμβάνουν ειδοποιήσεις προώθησης από GCM. Πρέπει επίσης να καθορίσετε το URI διανομείς ειδοποίηση Azure όπου η εφαρμογή Chrome θα πραγματοποιήσω μια κλήση ΥΠΌΛΟΙΠΟ για την καταχώρηση.
    Εφαρμογή μας δείγμα χρησιμοποιεί επίσης ένα αρχείο, το εικονίδιο `gcm_128.png`, που θα βρείτε στο αρχείο προέλευσης που χρησιμοποιείται ξανά από το αρχικό δείγμα GCM. Μπορείτε να το αντικαταστήσετε για οποιαδήποτε εικόνα που ταιριάζει με το [εικονίδιο κριτήρια](https://developer.chrome.com/apps/manifest/icons).

4. Δημιουργήστε ένα αρχείο που ονομάζεται `background.js` με τον ακόλουθο κώδικα:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Αυτό είναι το αρχείο που αναδύεται παραθύρου Chrome App HTML (**register.html**) και επίσης ορίζει το πρόγραμμα χειρισμού **messageReceived** να χειρίζεται τις εισερχόμενες push ειδοποίηση.

5. Δημιουργήστε ένα αρχείο που ονομάζεται `register.html` -αυτό ορίζει το περιβάλλον εργασίας Χρήστη της εφαρμογής Chrome. 

   >[AZURE.NOTE] Αυτό το δείγμα χρησιμοποιεί **CryptoJS v3.1.2**. Εάν έχετε κάνει λήψη σε μια άλλη έκδοση της βιβλιοθήκης, βεβαιωθείτε ότι θα αντικαταστήσετε σωστά στην έκδοση στο το `src` διαδρομή.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Δημιουργήστε ένα αρχείο που ονομάζεται `register.js` με τον παρακάτω κώδικα. Αυτό το αρχείο Καθορίζει τη δέσμη ενεργειών πίσω από `register.html`. Chrome εφαρμογές δεν επιτρέπουν την ενσωματωμένη εκτέλεσης, ώστε να χρειάζεται να δημιουργήσετε μια δέσμη ενεργειών ξεχωριστά τη δημιουργία αντιγράφων ασφαλείας για το περιβάλλον εργασίας Χρήστη.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Η παραπάνω δέσμη ενεργειών περιλαμβάνει τις ακόλουθες παραμέτρους κλειδιού:
    - **Window.onLoad** ορίζει τα συμβάντα κάντε κλικ στο κουμπί από τα δύο κουμπιά για το περιβάλλον εργασίας Χρήστη. Μία καταγράφει με GCM και το άλλο χρησιμοποιεί το Αναγνωριστικό καταχώρησης που επιστρέφεται μετά την καταχώρηση με GCM για την καταχώρηση με Azure ειδοποίηση διανομείς.
    - **updateLog** είναι η συνάρτηση που σας επιτρέπει να χειριστείτε δυνατότητες απλής καταγραφής.
    - **registerWithGCM** είναι το πρώτο κάντε κλικ στο κουμπί πρόγραμμα χειρισμού, οπότε το `chrome.gcm.register` κλήση GCM για να καταχωρήσετε την τρέχουσα παρουσία εφαρμογή Chrome.
    - **registerCallback** είναι η συνάρτηση επιστροφής κλήσης που λαμβάνει κλήση όταν η κλήση δήλωσης GCM επιστρέφει τιμή.
    - **registerWithNH** είναι το πρόγραμμα χειρισμού κάντε κλικ στο κουμπί που δεύτερο καταγράφει με ειδοποίηση διανομείς. Το λαμβάνει `hubName` και `connectionString` (το οποίο ο χρήστης έχει καθοριστεί) και crafts την κλήση ειδοποίηση διανομείς δήλωσης REST API.
    - **splitConnectionString** και **generateSaSToken** είναι βοηθητικά προγράμματα που αντιπροσωπεύει την εκτέλεση JavaScript μιας διαδικασίας διακριτικού δημιουργίας συσχετισμών ασφαλείας, που πρέπει να χρησιμοποιηθεί σε όλες τις κλήσεις REST API. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Συνήθεις έννοιες](http://msdn.microsoft.com/library/dn495627.aspx).
    - **sendNHRegistrationRequest** είναι η λειτουργία που πραγματοποιεί μια HTTP (REST) κλήσεων με Azure ειδοποίηση διανομείς.
    - **registrationPayload** Καθορίζει το φορτίο XML εγγραφής. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία δήλωσης NH REST API]. Ενημερώνουμε το Αναγνωριστικό καταχώρησης του με το τι θα σας λαμβάνονται από GCM.
    - **πρόγραμμα-πελάτη** είναι μια παρουσία του **XMLHttpRequest** που χρησιμοποιούμε για να κάνετε την αίτηση HTTP POST. Σημείωση που θα σας ενημερώσει το `Authorization` κεφαλίδας με `sasToken`. Επιτυχή ολοκλήρωση αυτής της κλήσης θα καταχώρηση αυτήν την παρουσία εφαρμογή Chrome Azure ειδοποίηση διανομείς.


Η συνολική δομή φακέλου για αυτό το έργο θα πρέπει να μοιάζει με αυτό:     ![Google Chrome εφαρμογή - δομής των φακέλων][21]

###<a name="set-up-and-test-your-chrome-app"></a>Ρύθμιση και δοκιμή της εφαρμογής σας Chrome

1. Ανοίξτε το πρόγραμμα περιήγησης Chrome. Ανοίξτε το **Chrome επεκτάσεις** και να ενεργοποιήσετε την **κατάσταση λειτουργίας προγραμματιστή**.

    ![Το Google Chrome - ενεργοποίηση της λειτουργίας προγραμματιστή][16]

2. Κάντε κλικ στην επιλογή **Φόρτωση μη συσκευασμένη επέκταση** και μεταβείτε στο φάκελο όπου δημιουργήσατε τα αρχεία. Επίσης, προαιρετικά, μπορείτε να χρησιμοποιήσετε το **Chrome εφαρμογές και επεκτάσεις προγραμματιστή εργαλείο**. Αυτό το εργαλείο είναι μια εφαρμογή Chrome στο ίδιο (εγκατεστημένο από το χώρο αποθήκευσης Web Chrome) και παρέχει σύνθετες δυνατότητες εντοπισμού σφαλμάτων για την ανάπτυξη εφαρμογών Chrome.

    ![Το Google Chrome - φόρτωση της μη συσκευασμένη επέκτασης][17]

3. Εάν η εφαρμογή Chrome δημιουργείται χωρίς σφάλματα, θα δείτε την εφαρμογή σας Chrome που εμφανίζονται.

    ![Το Google Chrome - εμφάνιση App Chrome][18]

4. Εισαγάγετε τον **Αριθμό έργου** που λάβατε νωρίτερα από την **Κονσόλα Google Cloud** όπως το Αναγνωριστικό του αποστολέα και κάντε κλικ στην επιλογή **καταχώρηση GCM**. Πρέπει να δείτε το μήνυμα **εγγραφής με GCM ολοκληρώθηκε με επιτυχία.**

    ![Το Google Chrome - Chrome εφαρμογή προσαρμογής][19]

5. Πληκτρολογήστε το **Όνομα διανομέας ειδοποίησης** και **DefaultListenSharedAccessSignature** που έχετε λάβει από την πύλη νωρίτερα και κάντε κλικ στην επιλογή **καταχώρηση Azure ειδοποίηση διανομέα**. Πρέπει να δείτε το μήνυμα **επιτυχής εγγραφή διανομέα της ειδοποίησης!** και τις λεπτομέρειες της απόκρισης εγγραφής, που περιέχει την καταχώρηση διανομείς ειδοποίηση Azure ID σας.

    ![Το Google Chrome - Καθορίστε λεπτομέρειες διανομέα ειδοποίησης][20]  

##<a name="send"></a>Στείλτε μια ειδοποίηση για την εφαρμογή του Chrome

Για σκοπούς δοκιμής, θα στείλουμε Chrome τις ειδοποιήσεις push, χρησιμοποιώντας μια .NET console εφαρμογής. 

>[AZURE.NOTE] Μπορείτε να στείλετε τις ειδοποιήσεις push με διανομείς ειδοποίηση από οποιαδήποτε παρασκηνίου μέσω μας δημόσια <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">ΥΠΌΛΟΙΠΑ περιβάλλοντος εργασίας</a>. Ανατρέξτε στο θέμα μας [πύλη τεκμηρίωση](https://azure.microsoft.com/documentation/services/notification-hubs/) για περισσότερα παραδείγματα πλατφόρμες.

1. Στο Visual Studio, από το μενού **αρχείο** , επιλέξτε **Δημιουργία** και, στη συνέχεια, το **Project**. Στην περιοχή **Visual C#**, κάντε κλικ στην επιλογή **Windows** και **Εφαρμογής κονσόλας**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.  Αυτό δημιουργεί ένα νέο έργο εφαρμογής κονσόλας.

2. Από το μενού **Εργαλεία** , κάντε κλικ στην επιλογή **Διαχείριση πακέτου βιβλιοθήκη** και, στη συνέχεια, **Κονσόλα διαχείρισης πακέτου**. Εμφανίζει την Κονσόλα διαχείρισης πακέτου.

3. Στο παράθυρο της κονσόλας, εκτελέστε την ακόλουθη εντολή:

        Install-Package Microsoft.Azure.NotificationHubs

    Αυτό προσθέτει μια αναφορά στο SDK Azure Service Bus με το <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">πακέτο WindowsAzure.ServiceBus NuGet</a>.

4. Άνοιγμα `Program.cs` και προσθέστε το εξής `using` δήλωση:

        using Microsoft.Azure.NotificationHubs;

5. Στο το `Program` κλάση, προσθέστε την ακόλουθη μέθοδο:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Βεβαιωθείτε ότι για να αντικαταστήσετε το `<hub name>` σύμβολο κράτησης θέσης με το όνομα του διανομέα ειδοποίηση που εμφανίζεται στην [πύλη](https://portal.azure.com) στο σας blade διανομέα ειδοποίησης. Επίσης, αντικαταστήστε το κείμενο κράτησης θέσης συμβολοσειρά σύνδεσης με τη συμβολοσειρά σύνδεσης που ονομάζεται `DefaultFullSharedAccessSignature` που έχετε λάβει στην ενότητα Ρύθμιση παραμέτρων ειδοποιήσεων διανομέα.

    >[AZURE.NOTE] Βεβαιωθείτε ότι χρησιμοποιείτε τη συμβολοσειρά σύνδεσης με **πλήρη** πρόσβαση, δεν **ακούτε** πρόσβασης. Τη συμβολοσειρά σύνδεσης **ακρόαση** access δεν εκχωρείτε δικαιώματα για την αποστολή ειδοποιήσεων push.

5. Προσθέστε τις ακόλουθες κλήσεις στο το `Main` μέθοδο:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Βεβαιωθείτε ότι εκτελείται το Chrome και εκτελέστε την εφαρμογή κονσόλας.

7. Θα πρέπει να βλέπετε την ακόλουθη ειδοποίηση αναδυόμενης στην επιφάνεια εργασίας σας.

    ![Το Google Chrome - ειδοποίηση][13]

8. Μπορείτε επίσης να δείτε όλες τις ειδοποιήσεις σας χρησιμοποιώντας το παράθυρο ειδοποιήσεις Chrome στη γραμμή εργασιών (στα Windows) όταν εκτελείται το Chrome.

    ![Το Google Chrome - λίστα ειδοποιήσεων][14]

>[AZURE.NOTE] Δεν χρειάζεται να έχετε την εφαρμογή Chrome εκτέλεση ή άνοιγμα στο πρόγραμμα περιήγησης (αν και πρέπει να εκτελεί το ίδιο το πρόγραμμα περιήγησης Chrome). Μπορείτε επίσης να λάβετε μια ενοποιημένη προβολή των όλες οι ειδοποιήσεις σας στο παράθυρο ειδοποιήσεις Chrome.

## <a name="next-steps"> </a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με την ειδοποίηση διανομείς στην [Ειδοποίηση Επισκόπηση διανομείς].

Για να στοχεύετε σε συγκεκριμένους χρήστες, ανατρέξτε το πρόγραμμα εκμάθησης [Azure ειδοποίηση διανομείς ειδοποιήστε χρήστες] . 

Εάν θέλετε να χωρίσετε τους χρήστες σας από τις ομάδες που σας ενδιαφέρουν, μπορείτε να ακολουθήσετε το πρόγραμμα εκμάθησης [Azure ειδοποίηση διανομείς διακοπή συζητήσεων] .

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Δείγμα διανομέα ειδοποίηση εφαρμογής Chrome]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Κονσόλα Google Cloud]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Επισκόπηση διανομείς ειδοποίησης]: notification-hubs-push-notification-overview.md
[Επισκόπηση εφαρμογές Chrome]: https://developer.chrome.com/apps/about_apps
[Δείγμα εφαρμογής GCM Chrome]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome εφαρμογών στο κινητό]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Δημιουργία καταχώρησης NH REST API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[βιβλιοθήκη Crypto js]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud ανταλλαγής μηνυμάτων για το Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Ειδοποίηση Azure διανομείς ειδοποιήστε τους χρήστες]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure διανομείς ειδοποίηση πρόσφατες ειδήσεις]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

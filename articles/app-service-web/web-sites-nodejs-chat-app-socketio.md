<properties
    pageTitle="Δημιουργία μιας εφαρμογής συνομιλίας Node.js με Socket.IO στο Azure εφαρμογής υπηρεσίας"
    description="Ένα πρόγραμμα εκμάθησης που δείχνει τη χρήση socket.io σε μια εφαρμογή web node.js που φιλοξενούνται στο Azure."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Δημιουργία μιας εφαρμογής συνομιλίας Node.js με Socket.IO στο Azure εφαρμογής υπηρεσίας

Socket.IO παρέχει σε πραγματικό χρόνο επικοινωνία μεταξύ του διακομιστή node.js και προγράμματα-πελάτες χρησιμοποιώντας WebSockets. Επίσης, υποστηρίζει επιστροφή σε άλλες μεταφορές (όπως μεγάλη σταθμοσκόπησης) που λειτουργούν με παλαιότερα προγράμματα περιήγησης. Αυτό το πρόγραμμα εκμάθησης θα σας καθοδηγήσει φιλοξενίας μιας εφαρμογής Socket.IO βάσει συνομιλίας με Azure web app και δείχνουν τον τρόπο για να κλιμακωθεί την εφαρμογή χρησιμοποιώντας [Azure Redis Cache]. Για περισσότερες πληροφορίες σχετικά με Socket.IO, ανατρέξτε στο θέμα <http://socket.io/>.

> [AZURE.NOTE] Οι διαδικασίες σε αυτήν την εργασία εφαρμόζονται σε [Εφαρμογές Web της εφαρμογής υπηρεσίας]; για τις υπηρεσίες Cloud, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής Node.js συνομιλείτε με Socket.IO σε μια υπηρεσία Cloud Azure].

## <a name="download-the-chat-example"></a>Κάντε λήψη του παραδείγματος συνομιλίας

Για αυτό το έργο, θα χρησιμοποιήσουμε το παράδειγμα συνομιλίας από το [αποθετήριο Socket.IO GitHub]. Ακολουθήστε τα παρακάτω βήματα για να κάνετε λήψη του παραδείγματος και να την προσθέσετε στο έργο που δημιουργήσατε προηγουμένως.

1.  Κάντε λήψη ενός [ZIP ή GZ αρχειοθετηθούν τελική έκδοση] του έργου Socket.IO (έκδοση 1.3.5 χρησιμοποιήθηκε για αυτό το έγγραφο)

1.  Εξαγωγή του αρχειοθέτησης και αντιγράψτε την **Παραδείγματα\\συνομιλίας** καταλόγου σε νέα θέση. Για παράδειγμα, ** \\κόμβου\\συνομιλίας**.

## <a name="modify-appjs-and-install-modules"></a>Τροποποίηση app.js και εγκατάσταση λειτουργικές μονάδες

1.  Μετονομάστε το αρχείο **index.js** σε **app.js**. Αυτό σας επιτρέπει Azure για τον εντοπισμό ότι αυτή είναι μια εφαρμογή Node.js.

1.  Ανοίξτε το αρχείο **app.js** σε έναν επεξεργαστή κειμένου. Αλλάξτε τη γραμμή που περιέχει `var io = require('../..')(server);` όπως φαίνεται παρακάτω:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Ανοίξτε το αρχείο **package.json** και να προσθέσετε μια αναφορά σε socket.io στην περιοχή `dependencies`, όπως φαίνεται παρακάτω:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Από τη γραμμή εντολών, αλλάξτε σε το ** \\κόμβου\\συνομιλίας** καταλόγου και χρήση npm για να εγκαταστήσετε τις λειτουργικές μονάδες που απαιτούνται από αυτήν την εφαρμογή:

        npm install

    Αυτό θα εγκαταστήσει τις λειτουργικές μονάδες μέσα σε έναν υποφάκελο με το όνομα **node_modules**.

## <a name="create-an-azure-web-app"></a>Δημιουργία μιας εφαρμογής Azure Web

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια εφαρμογή Azure web, ενεργοποίηση Git δημοσίευσης και, στη συνέχεια, ενεργοποίηση WebSocket υποστήριξης για την εφαρμογή web.

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure δωρεάν δοκιμαστικής έκδοσης</a>.

1. Εγκαταστήστε το περιβάλλον γραμμής εντολών Azure (Azure CLI) και σύνδεση με τη συνδρομή σας στο Azure. Ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md).

1. Εάν αυτή είναι η ώρα πρώτη ρυθμίζετε ένα αποθετήριο στο Azure, πρέπει να δημιουργήσετε διαπιστευτηρίων σύνδεσης. Από το Azure CLI, εισαγάγετε την ακόλουθη εντολή:

        azure site deployment user set [username] [password]

1. Για να αλλάξετε το ** \\node\chat** καταλόγου και χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε μια νέα Azure web app και ένα τοπικό αρχείο φύλαξης Git. Αυτή η εντολή δημιουργεί επίσης ένα Git remote με όνομα 'azure'.

        azure site create mysitename --git

    Πρέπει να αντικαταστήσετε 'mysitename' με ένα μοναδικό όνομα για την εφαρμογή web.

1. Ολοκλήρωση των υπαρχόντων αρχείων στο τοπικό αρχείο φύλαξης, χρησιμοποιώντας τις παρακάτω εντολές:

        git add .
        git commit -m "Initial commit"

1. Push τα αρχεία του αποθετηρίου Azure Web Apps με την ακόλουθη εντολή:

        git push azure master

    Όταν σας ζητηθεί, εισαγάγετε τα διαπιστευτήριά σας από το βήμα 2. Λαμβάνετε μηνύματα κατάστασης όπως λειτουργικές μονάδες εισάγονται στο διακομιστή. Μόλις ολοκληρωθεί η διαδικασία αυτή, η εφαρμογή θα να φιλοξενηθούν σε εφαρμογή Azure web της.

    > [AZURE.NOTE] Κατά την εγκατάσταση της λειτουργικής μονάδας, ενδέχεται να παρατηρήσετε σφάλματα που ' το έργο που έχει εισαχθεί... δεν βρέθηκε ". Αυτά είναι ασφαλές να παραβλέψετε.

1. Socket.IO χρησιμοποιεί WebSockets, το οποίο δεν είναι ενεργοποιημένη από προεπιλογή σε Azure. Για να ενεργοποιήσετε web sockets, χρησιμοποιήστε την ακόλουθη εντολή:

        azure site set -w

    Εάν σας ζητηθεί, πληκτρολογήστε το όνομα της εφαρμογής web.

    >[AZURE.NOTE]
    >Η 'azure τοποθεσίας set -w' εντολή θα λειτουργούν μόνο με την έκδοση 0.7.4 ή νεότερη έκδοση της περιβάλλον γραμμής εντολών του Azure. Μπορείτε επίσης να ενεργοποιήσετε την υποστήριξη WebSocket με την [Πύλη Azure](https://portal.azure.com).
    >
    >Για να ενεργοποιήσετε WebSockets με την πύλη Azure, κάντε κλικ στην εφαρμογή web από το Web Apps blade, κάντε κλικ στην επιλογή **όλες οι ρυθμίσεις** > **Ρυθμίσεις εφαρμογής**. Στην περιοχή **Web Sockets**, κάντε κλικ **στο**. Στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

1. Για να δείτε την εφαρμογή web Azure, χρησιμοποιήστε την ακόλουθη εντολή για την εκκίνηση του προγράμματος περιήγησης web και μεταβείτε στην εφαρμογή web που φιλοξενείται:

        azure site browse

Η εφαρμογή εκτελείται τώρα στον Azure και μπορούν να μεταβιβάσουν μηνύματα συνομιλίας ανάμεσα σε διαφορετικούς υπολογιστές-πελάτες που χρησιμοποιούν Socket.IO.

## <a name="scale-out"></a>Διαβάθμιση

Εφαρμογές Socket.IO μπορεί έχει, χρησιμοποιώντας έναν __προσαρμογέα__ για τη διανομή μηνυμάτων και συμβάντα μεταξύ πολλών παρουσιών της εφαρμογής. Παρόλο που υπάρχουν αρκετές προσαρμογέων, τον προσαρμογέα [socket.io redis] μπορεί να χρησιμοποιηθεί εύκολα με τη δυνατότητα Azure Redis Cache.

> [AZURE.NOTE] Μια επιπλέον απαίτηση για κλιμάκωση ανάληψη μια λύση Socket.IO είναι η υποστήριξη για περιόδους λειτουργίας ασύγχρονα. Αυτοκόλλητες περίοδοι λειτουργίας είναι ενεργοποιημένες από προεπιλογή για τις εφαρμογές Web Azure μέσω του Azure αίτηση για τη δρομολόγηση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Παρουσία συσχέτισης σε τοποθεσίες Web Azure].

### <a name="create-a-redis-cache"></a>Δημιουργήστε ένα cache Redis

Ακολουθήστε τα βήματα στο θέμα [Δημιουργία μιας cache στο Azure Redis Cache] για να δημιουργήσετε ένα νέο cache.

> [AZURE.NOTE] Αποθηκεύστε το __όνομα κεντρικού υπολογιστή__ και το __πρωτεύον κλειδί__ για το cache, όπως αυτά θα χρειαστούν στα επόμενα βήματα.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Προσθέστε τις redis και socket.io redis λειτουργικές μονάδες

1. Από μια γραμμή εντολών, αλλάξτε σε το __ \\κόμβου\\συνομιλίας__ καταλόγου και χρησιμοποιήστε την ακόλουθη εντολή.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] Οι εκδόσεις που καθορίζονται σε αυτήν την εντολή είναι τις εκδόσεις που χρησιμοποιείται κατά τη δοκιμή αυτού του άρθρου.

1. Τροποποιήστε το αρχείο __app.js__ για να προσθέσετε τα ακόλουθα γραμμές αμέσως μετά`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Αντικατάσταση __redishostname__ και __rediskey__ με το όνομα κεντρικού υπολογιστή και το κλειδί για το cache Redis.

    Θα δημιουργήσετε μια δημοσίευση και εγγραφή προγράμματος-πελάτη στο cache Redis που δημιουργήσατε προηγουμένως. Τα προγράμματα-πελάτες, στη συνέχεια, που χρησιμοποιούνται με τον προσαρμογέα για να ρυθμίσετε τις παραμέτρους Socket.IO για να χρησιμοποιήσετε το cache Redis για μεταφορά μηνύματα και τα συμβάντα μεταξύ των παρουσιών της εφαρμογής σας

    > [AZURE.NOTE] Ενώ το προσαρμογέα __socket.io redis__ μπορούν να επικοινωνούν απευθείας στο Redis, την τρέχουσα έκδοση δεν υποστηρίζει τον έλεγχο ταυτότητας που απαιτείται από το Azure Redis cache. Επομένως, η αρχική σύνδεση δημιουργείται με χρήση της λειτουργικής μονάδας __redis__ και, στη συνέχεια, το πρόγραμμα-πελάτη που του μεταβιβάστηκε η προσαρμογέα __socket.io redis__ .
    >
    > Ενώ Azure Redis Cache υποστηρίζει ασφαλείς συνδέσεις χρησιμοποιώντας τη θύρα 6380, οι λειτουργικές μονάδες που χρησιμοποιούνται σε αυτό το παράδειγμα δεν υποστηρίζουν ασφαλείς συνδέσεις από 14/7/2014. Ο παραπάνω κώδικας χρησιμοποιεί την προεπιλεγμένη, μη ασφαλής θύρα της 6379.

1. Αποθηκεύστε το τροποποιημένο __app.js__

### <a name="commit-changes-and-redeploy"></a>Ολοκλήρωση αλλαγών και αναπτύξτε ξανά

Από τη γραμμή εντολών στο το __ \\κόμβου\\συνομιλίας__ καταλόγου, χρησιμοποιήστε τις παρακάτω εντολές για να πραγματοποιήσετε αλλαγές και αναπτύξτε ξανά την εφαρμογή.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Αφού έχετε έχουν προωθηθεί τις αλλαγές στο διακομιστή, μπορείτε να κλιμάκωση την τοποθεσία σας σε πολλές παρουσίες, χρησιμοποιώντας την ακόλουθη εντολή.

    azure site scale instances --instances #

Όπου __#__ είναι ο αριθμός των παρουσιών για να δημιουργήσετε.

Μπορείτε να συνδεθείτε σε εφαρμογή web από πολλά προγράμματα περιήγησης ή υπολογιστές για να επιβεβαιώσετε ότι τα μηνύματα αποστέλλονται σωστά σε όλους τους πελάτες.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

### <a name="connection-limits"></a>Όρια σύνδεσης

Azure Web Apps είναι διαθέσιμο σε πολλά SKU, που καθορίζουν τους πόρους που είναι διαθέσιμες στην τοποθεσία σας. Αυτό περιλαμβάνει τον αριθμό των επιτρεπόμενων WebSocket συνδέσεων. Για περισσότερες πληροφορίες, ανατρέξτε στη [σελίδα Τιμολόγηση εφαρμογές Web].

### <a name="messages-arent-being-sent-using-websockets"></a>Τα μηνύματα δεν αποστέλλεται χρησιμοποιώντας WebSockets

Εάν τα προγράμματα περιήγησης προγράμματος-πελάτη διατήρηση πτώσης ξανά για να χρονικό διάστημα σταθμοσκόπησης αντί για τη χρήση WebSockets, μπορεί να είναι λόγω ένα από τα εξής.

* **Δοκιμάστε να περιορίσετε τη μεταφορά τους προς απλώς WebSockets**

    Με τη σειρά για Socket.IO για να χρησιμοποιήσετε WebSockets ως τη μεταφορά μηνυμάτων, ο διακομιστής και ο υπολογιστής-πελάτης πρέπει να υποστηρίζει WebSockets. Εάν μία ή την άλλη δεν έχει, Socket.IO θα διαπραγμάτευσης άλλο μεταφοράς, όπως μεγάλη ψηφοφορίας. Η προεπιλεγμένη λίστα μεταφορών που χρησιμοποιείται από το Socket.IO είναι ` websocket, htmlfile, xhr-polling, jsonp-polling`. Μπορείτε να επιβάλετε για να χρησιμοποιήσετε μόνο WebSockets, προσθέτοντας τον παρακάτω κώδικα στο αρχείο **app.js** , μετά από τη γραμμή που περιέχει `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Σημειώστε ότι δεν θα μπορείτε να συνδεθείτε στην τοποθεσία με ενεργοποιημένο τον παραπάνω κώδικα, όπως το περιορίζει την επικοινωνία με το WebSockets μόνο παλαιότερα προγράμματα περιήγησης που δεν υποστηρίζουν WebSockets.

* **Χρήση SSL**

    WebSockets βασίζεται σε ορισμένες μικρότερο βαθμό που χρησιμοποιήθηκαν HTTP κεφαλίδες, όπως την κεφαλίδα **αναβάθμιση** . Ορισμένες συσκευές ενδιάμεσου δικτύου, όπως διακομιστές μεσολάβησης web, ενδέχεται να καταργήσει αυτές τις κεφαλίδες. Για να αποφύγετε αυτό το πρόβλημα, μπορείτε να δημιουργήσετε τη σύνδεση WebSocket μέσω SSL.

    Ένας εύκολος τρόπος για να γίνει αυτό είναι να ρυθμίσετε τις παραμέτρους Socket.IO για να `match origin protocol`. Αυτό καθοδηγεί Socket.IO για την ασφάλιση επικοινωνίας WebSockets η ίδια με την αρχική αίτηση HTTP/HTTPS για την ιστοσελίδα. Εάν ένα πρόγραμμα περιήγησης χρησιμοποιεί μια διεύθυνση URL HTTPS για να επισκεφθείτε την τοποθεσία Web σας, οι επόμενες WebSocket επικοινωνιών μέσω Socket.IO θα είναι ασφαλείς μέσω SSL.

    Για να τροποποιήσετε αυτό το παράδειγμα για να ενεργοποιήσετε αυτήν τη ρύθμιση παραμέτρων, προσθέστε τον ακόλουθο κώδικα στο αρχείο **app.js** μετά τη γραμμή που περιέχει `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Επαληθεύστε τις ρυθμίσεις web.config**

    Εφαρμογές Azure web που φιλοξενούν εφαρμογές Node.js Χρησιμοποιήστε το αρχείο **web.config** για τη δρομολόγηση εισερχόμενες αιτήσεις στην εφαρμογή Node.js. Για WebSockets για να λειτουργήσει σωστά με εφαρμογές Node.js, το **web.config** πρέπει να περιέχει την ακόλουθη καταχώρηση.

        <webSocket enabled="false"/>

    Αυτό απενεργοποιεί τη λειτουργική μονάδα WebSockets των υπηρεσιών IIS, που περιλαμβάνει τη δική του υλοποίηση WebSockets και έρχεται σε διένεξη με συγκεκριμένες ενότητες WebSocket Node.js όπως Socket.IO. Εάν δεν υπάρχει αυτήν τη γραμμή ή έχει οριστεί σε `true`, αυτό μπορεί να είναι το λόγο για τον οποίο η μεταφορά WebSocket δεν λειτουργεί για την εφαρμογή σας.

    Κανονικά, Node.js εφαρμογές δεν περιλαμβάνει ένα αρχείο **web.config** , ώστε να Azure τοποθεσίες Web δημιουργεί αυτόματα ένα για εφαρμογές Node.js όταν αναπτύσσονται. Επειδή αυτό το αρχείο δημιουργείται αυτόματα στο διακομιστή, πρέπει να χρησιμοποιήσετε το FTP ή FTPS μιας διεύθυνσης URL για την τοποθεσία Web για να δείτε αυτό το αρχείο. Μπορείτε να βρείτε το FTP και FTPS διευθύνσεις URL για την τοποθεσία σας στην πύλη του κλασική, επιλέγοντας την εφαρμογή web της και, στη συνέχεια, τη σύνδεση **πίνακα εργαλείων** . Οι διευθύνσεις URL που εμφανίζονται στην ενότητα **γρήγορη ματιά** .

    > [AZURE.NOTE] Το αρχείο **web.config** δημιουργείται μόνο από τοποθεσίες Web Azure, εάν η εφαρμογή σας δεν παρέχει ένα. Εάν παρέχεται ένα αρχείο **web.config** στον ριζικό κατάλογο του έργου σας εφαρμογής, θα χρησιμοποιηθεί από εφαρμογές Web Azure.

    Εάν η καταχώρηση δεν υπάρχει ή έχει οριστεί στην τιμή `true`, στη συνέχεια, πρέπει να δημιουργήσετε μια **web.config** στον ριζικό κατάλογο της εφαρμογής σας Node.js και να καθορίσετε μια τιμή του `false`.  Για αναφορά, τα παρακάτω είναι μια προεπιλεγμένη **web.config** για μια εφαρμογή που χρησιμοποιεί **app.js** ως το σημείο εισόδου.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Εάν η εφαρμογή σας χρησιμοποιεί ένα σημείο εισόδου εκτός από το **app.js**, πρέπει να αντικαταστήσετε όλες τις εμφανίσεις του **app.js** με το σημείο εισόδου σωστά. Για παράδειγμα, αντικαθιστώντας **app.js** με **server.js**.

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας], όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης μάθατε πώς μπορείτε να δημιουργήσετε μια εφαρμογή συνομιλίας που φιλοξενούνται σε μια εφαρμογή Azure web. Μπορείτε επίσης να φιλοξενήσετε αυτήν την εφαρμογή ως υπηρεσία Cloud Azure. Για οδηγίες σχετικά με τον τρόπο για να γίνει αυτό, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής Node.js συνομιλείτε με Socket.IO σε μια υπηρεσία Cloud Azure].

Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές του Node.js].

## <a name="whats-changed"></a>Τι έχει αλλάξει

* Για οδηγίες για την αλλαγή από τοποθεσίες Web App υπηρεσία ανατρέξτε στο θέμα: [Azure εφαρμογής υπηρεσίας και τον αντίκτυπο σχετικά με τις υπάρχουσες υπηρεσίες Azure].

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[Εφαρμογές Web της εφαρμογής υπηρεσίας]: http://go.microsoft.com/fwlink/?LinkId=529714
[Σελίδα Τιμολόγηση εφαρμογών Web]: http://go.microsoft.com/fwlink/?LinkId=511643
[Δημιουργία μιας εφαρμογής Node.js συνομιλίας με Socket.IO σε μια υπηρεσία Azure Cloud]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure εφαρμογής υπηρεσίας και τις επιπτώσεις της σχετικά με τις υπάρχουσες υπηρεσίες Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Κέντρο για προγραμματιστές του node.js]: /develop/nodejs/
[Δοκιμάστε εφαρμογής υπηρεσίας]: http://go.microsoft.com/fwlink/?LinkId=523751
[Συσχέτιση παρουσία στη Azure τοποθεσίες Web]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Δημιουργήστε ένα cache στο Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.IO redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub αποθετήριο δεδομένων]: https://github.com/socketio/socket.io
[ZIP ή GZ αρχειοθετηθούν κυκλοφορίας]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png

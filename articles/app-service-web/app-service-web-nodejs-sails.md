<properties
    pageTitle="Ανάπτυξη εφαρμογής web Sails.js να Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να αναπτύξετε μια εφαρμογή Node.js Azure εφαρμογής υπηρεσίας. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να αναπτύξετε μια εφαρμογή web Sails.js."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Ανάπτυξη εφαρμογής web Sails.js να Azure εφαρμογής υπηρεσίας

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να αναπτύξετε μια εφαρμογή Sails.js σε Azure εφαρμογής υπηρεσίας. Κατά τη διαδικασία, μπορείτε να glean ορισμένες γενικές γνώσεις σχετικά με τον τρόπο για να ρυθμίσετε την εφαρμογή σας Node.js για να εκτελέσετε σε εφαρμογή υπηρεσίας. 

Θα πρέπει να διαθέτετε εμπειρία του Sails.js. Αυτό το πρόγραμμα εκμάθησης δεν προορίζεται για να σας βοηθήσει με θέματα που σχετίζονται με την εκτέλεση Sail.js Γενικά.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Git](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- Ένας λογαριασμός Microsoft Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να [εγγραφείτε για μια δωρεάν δοκιμαστική έκδοση](/pricing/free-trial/?WT.mc_id=A261C142F) ή να [ενεργοποιήσετε το Visual Studio πλεονεκτήματα συνδρομητών](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Για να δείτε Azure εφαρμογής υπηρεσίας στην πράξη πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751). Εκεί, μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή μικρής διάρκειας starter στην εφαρμογή υπηρεσίας — απαιτείται πιστωτική κάρτα, χωρίς δεσμεύσεις.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Βήμα 1: Δημιουργία εφαρμογής Sails.js τοπικά

Πρώτα, δημιουργήστε γρήγορα μια προεπιλεγμένη Sails.js εφαρμογή στο περιβάλλον ανάπτυξής σας, ακολουθώντας τα παρακάτω βήματα:

1. Ανοίξτε το terminal γραμμής εντολών της επιλογής σας και `CD` σε έναν κατάλογο εργασίας.

2. Δημιουργία εφαρμογής Sails.js και εκτελέστε το:

        sails new <appname>
        cd <appname>
        sails lift

    Βεβαιωθείτε ότι μπορείτε να μεταβείτε στην αρχική σελίδα του προεπιλεγμένου στο http://localhost:1377.

## <a name="step-2-create-the-azure-app-resource"></a>Βήμα 2: Δημιουργία του πόρου Azure εφαρμογής

Στη συνέχεια, δημιουργήστε τον πόρο εφαρμογής υπηρεσίας στο Azure. Πρόκειται για την ανάπτυξη της εφαρμογής σας Sails.js σε αυτό αργότερα.

1. συνδεθείτε στο Azure παρόμοια ώστε:
1. Στο ίδιο το terminal, αλλάξτε σε κατάσταση λειτουργίας ASM και συνδεθείτε στο Azure:

        azure config mode asm
        azure login

    Ακολουθήστε το προτροπή για να συνεχίσετε τη σύνδεση σε ένα πρόγραμμα περιήγησης με ένα λογαριασμό Microsoft που διαθέτει τη συνδρομή σας στο Azure.

2. Βεβαιωθείτε ότι βρίσκεστε ακόμη στον ριζικό κατάλογο του έργου σας Sails.js. Δημιουργία του πόρου εφαρμογή εφαρμογής υπηρεσίας στο Azure με εφαρμογή μοναδικό όνομα με την επόμενη εντολή. Η διεύθυνση URL της εφαρμογής σας web είναι http://&lt;Όνομα_εφαρμογής >. azurewebsites.net.

        azure site create --git <appname>

    Ακολουθήστε το μήνυμα για να επιλέξετε μια περιοχή Azure για ανάπτυξη. Εάν έχετε ποτέ ορίζετε Git/FTP ανάπτυξης διαπιστευτήρια για τη συνδρομή σας Azure, επίσης θα σας ζητηθεί να τα δημιουργήσετε.

    Όταν δημιουργηθεί ο πόρος εφαρμογή εφαρμογής υπηρεσίας:

    - Η εφαρμογή Sails.js είναι προετοιμασία Git
    - Το τοπικό αποθετήριο προετοιμασία Git είναι συνδεδεμένη με τη νέα εφαρμογή εφαρμογής υπηρεσίας ως μια απομακρυσμένη, Git κατάλληλα με το όνομα "azure", και
    - Και δημιουργείται αρχείο iisnode.yml του ριζικού καταλόγου. Μπορείτε να χρησιμοποιήσετε αυτό το αρχείο για να ρυθμίσετε τις παραμέτρους [iisnode](https://github.com/tjanczuk/iisnode), που χρησιμοποιεί εφαρμογής υπηρεσίας για να εκτελέσετε Node.js εφαρμογές.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Βήμα 3: Ρύθμιση παραμέτρων και την ανάπτυξη της εφαρμογής σας Sails.js

 Εργασία με μια εφαρμογή Sails.js στην εφαρμογή υπηρεσίας αποτελείται από τρία βασικά βήματα:

 - Ρύθμιση παραμέτρων της εφαρμογής για το για να εκτελέσετε σε εφαρμογής υπηρεσίας
 - Ανάπτυξη εφαρμογής υπηρεσίας
 - Διαβάστε stderr stdout αρχεία καταγραφής και για την αντιμετώπιση τυχόν προβλημάτων ανάπτυξης

Ακολουθήστε τα παρακάτω βήματα:

1. Ανοίξτε το νέο αρχείο iisnode.yml του ριζικού καταλόγου και προσθέστε τις ακόλουθες δύο γραμμές:

        loggingEnabled: true
        logDirectory: iisnode

    Καταγραφή τώρα είναι ενεργοποιημένη για iisnode. Για περισσότερες πληροφορίες σχετικά με πώς λειτουργεί αυτό, ανατρέξτε στο θέμα  [λήψη stdout και stderr αρχεία καταγραφής από iisnode](app-service-web-nodejs-get-started.md#iisnodelog).

2. Άνοιγμα config/env/production.js να ρυθμίσει το περιβάλλον παραγωγής και να ορίσετε `port` και `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Μπορείτε να βρείτε την τεκμηρίωση για αυτές τις ρυθμίσεις παραμέτρων στην  [Τεκμηρίωση Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Στη συνέχεια, πρέπει να βεβαιωθείτε ότι είναι συμβατό με τις μονάδες δίσκου δικτύου του Azure [Grunt](https://www.npmjs.com/package/grunt) . Εκδόσεις grunt μικρότερο του 1.0.0 χρησιμοποιεί ένα πακέτο μη ενημερωμένων [glob](https://www.npmjs.com/package/glob) (λιγότερο από 5.0.14), που δεν υποστηρίζει μονάδες δίσκου δικτύου. 

3. Ανοίξτε package.json και αλλάξτε το `grunt` έκδοση για να `1.0.0` και να καταργήσετε όλα `grunt-*` πακέτων. Σας `dependencies` ιδιότητα πρέπει να μοιάζει ως εξής:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Στο package.json, προσθέστε τα ακόλουθα `engines` ιδιοτήτων για να ορίσετε την έκδοση Node.js σε μία που θέλετε.

        "engines": {
            "node": "6.6.0"
        },

6. Αποθηκεύστε τις αλλαγές σας και να εξετάσετε τις αλλαγές σας για να βεβαιωθείτε ότι την εφαρμογή σας εξακολουθούν να εκτελείται τοπικά. Για να το κάνετε αυτό, διαγράψτε το `node_modules` φακέλου και, στη συνέχεια, εκτελέστε:

        npm install
        sails lift

4. Τώρα, μπορείτε να χρησιμοποιήσετε git για να αναπτύξετε την εφαρμογή σας Azure:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Τέλος, απλώς εκκινήστε την ζωντανή Azure εφαρμογή στο πρόγραμμα περιήγησης:

        azure site browse

    Τώρα θα πρέπει να βλέπετε την ίδια Sails.js αρχική σελίδα.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Αντιμετώπιση προβλημάτων με την ανάπτυξη

Εάν αποτύχει η εφαρμογή σας Sails.js για κάποιο λόγο στην εφαρμογή υπηρεσίας, βρείτε τα αρχεία καταγραφής stderr για να βοηθήσουν στην αντιμετώπιση του.
Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [λήψη stdout και stderr αρχεία καταγραφής από iisnode](app-service-web-nodejs-sails.md#iisnodelog).
Εάν έχει ήδη ξεκινήσει με επιτυχία, το αρχείο καταγραφής stdout θα πρέπει να εμφανίζουν το οικείο μήνυμα:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Μπορείτε να ελέγχετε υποδιαίρεση της τα αρχεία καταγραφής stdout στο αρχείο [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) . 

## <a name="connect-to-a-database-in-azure"></a>Σύνδεση με βάση δεδομένων στο Azure

Για να συνδεθείτε με μια βάση δεδομένων στο Azure, δημιουργείτε τη βάση δεδομένων της επιλογής σας στο Azure, όπως η βάση δεδομένων SQL Azure, MySQL, MongoDB, Cache Azure (Redis), κ.λπ., και χρησιμοποιήστε το αντίστοιχο [προσαρμογέα αποθήκευσης δεδομένων](https://github.com/balderdashy/sails#compatibility) για να συνδεθείτε σε αυτήν. Τα βήματα σε αυτήν την ενότητα σας δείχνουν πώς μπορείτε να συνδεθείτε με μια βάση δεδομένων MySQL στο Azure.

1. Ακολουθήστε τα εκπαιδευτικά [εδώ](../store-php-create-mysql-database.md) για να δημιουργήσετε μια βάση δεδομένων MySQL στο Azure.

2. Από το terminal γραμμής εντολών, εγκαταστήστε τον προσαρμογέα MySQL:

        npm install sails-mysql --save

3. Ανοίξτε config/connections.js και προσθέστε το ακόλουθο αντικείμενο σύνδεσης στη λίστα: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Για κάθε μεταβλητή περιβάλλοντος (`process.env.*`), πρέπει να τη ρυθμίσετε στην εφαρμογή υπηρεσίας. Για να το κάνετε αυτό, εκτελέστε τις ακόλουθες εντολές από τερματικού σας. Όλες τις πληροφορίες σύνδεσης πρέπει να είναι στην πύλη του Azure (ανατρέξτε στο θέμα [σύνδεση με τη βάση δεδομένων MySQL](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Με τις ρυθμίσεις στις ρυθμίσεις του Azure app διατηρεί ευαίσθητα δεδομένα από το στοιχείο ελέγχου προέλευσης (Git). Στη συνέχεια, θα μπορείτε να ρυθμίσετε το περιβάλλον ανάπτυξης για να χρησιμοποιήσετε τις ίδιες πληροφορίες σύνδεσης.

4. Ανοίξτε config/local.js και προσθέστε το ακόλουθο αντικείμενο συνδέσεις:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Αυτή η ρύθμιση αντικαθιστά τις ρυθμίσεις στο αρχείο σας config/connections.js για το τοπικό περιβάλλον. Αυτό το αρχείο εξαιρείται από την προεπιλεγμένη .gitignore στο έργο σας, έτσι δεν θα αποθηκευτεί στο Git. Τώρα, θα μπορείτε να συνδεθείτε με τη βάση δεδομένων MySQL τόσο από την εφαρμογή Azure web και από το περιβάλλον τοπικής ανάπτυξης.

4. Ανοίξτε το config/env/production.js για να ρυθμίσετε το περιβάλλον παραγωγής σας και προσθέστε τα ακόλουθα `models` αντικειμένου:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Ανοίξτε το config/env/development.js για να ρυθμίσετε το περιβάλλον ανάπτυξης και προσθέστε τα ακόλουθα `models` αντικειμένου:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`σας επιτρέπει να χρησιμοποιείτε δυνατότητες μετεγκατάσταση βάσης δεδομένων για να δημιουργήσετε και να ενημερώσετε τους πίνακες βάσης δεδομένων σε σας MySQL εύκολα. Ωστόσο, `migrate: 'safe'` χρησιμοποιείται για το περιβάλλον του Azure (παραγωγή) επειδή Sails.js δεν σας επιτρέπει να χρησιμοποιήσετε `migrate: 'alter'` σε ένα περιβάλλον παραγωγής (ανατρέξτε στην  [Τεκμηρίωση Sails.js](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Από το terminal, [δημιουργούν](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) ένα Sails.js [API σχέδιο](http://sailsjs.org/documentation/concepts/blueprints) όπως θα κάνατε, στη συνέχεια, εκτελούνται κανονικά `sails lift` για να δημιουργήσετε τη βάση δεδομένων με Sails.js μετεγκατάσταση της βάσης δεδομένων. Για παράδειγμα:

         sails generate api mywidget
         sails lift

    Το `mywidget` μοντέλο που δημιουργούνται από αυτήν την εντολή είναι κενό, αλλά θα σας να το χρησιμοποιήσετε για την εμφάνιση που έχουμε συνδεσιμότητα βάσεων δεδομένων.
    Κατά την εκτέλεση `sails lift`, που δημιουργεί τους πίνακες που λείπουν για τα μοντέλα την εφαρμογή σας χρησιμοποιεί.

6. Αποκτήστε πρόσβαση στο το σχέδιο API που μόλις δημιουργήσατε στο πρόγραμμα περιήγησης. Για παράδειγμα:

        http://localhost:1337/mywidget/create
    
    Το API πρέπει να επιστρέψετε στην εγγραφή που έχουν δημιουργηθεί σε εσάς στο παράθυρο του προγράμματος περιήγησης, γεγονός που σημαίνει ότι η βάση δεδομένων είναι δημιουργήθηκε με επιτυχία.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Τώρα, προωθήσετε τις αλλαγές σας σε Azure και αναζητήστε την εφαρμογή σας για να βεβαιωθείτε ότι εξακολουθεί να λειτουργεί.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Αποκτήστε πρόσβαση στο το σχέδιο API της εφαρμογής Azure web. Για παράδειγμα:

        http://<appname>.azurewebsites.net/mywidget/create

    Εάν το API επιστρέφει μια άλλη νέα εγγραφή, στη συνέχεια, την εφαρμογή Azure web μιλάει στη βάση δεδομένων MySQL.

## <a name="more-resources"></a>Περισσότεροι πόροι

- [Γρήγορα αποτελέσματα με το Node.js web apps στο Azure εφαρμογής υπηρεσίας](app-service-web-nodejs-get-started.md)
- [Χρήση Node.js λειτουργικές μονάδες με εφαρμογές του Azure](../nodejs-use-node-modules-azure-apps.md)

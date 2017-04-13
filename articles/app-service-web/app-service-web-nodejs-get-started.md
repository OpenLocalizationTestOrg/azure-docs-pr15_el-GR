<properties
    pageTitle="Γρήγορα αποτελέσματα με το Node.js web apps στο Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να αναπτύξετε μια εφαρμογή Node.js για μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας."
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
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Γρήγορα αποτελέσματα με το Node.js web apps στο Azure εφαρμογής υπηρεσίας

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε μια απλή εφαρμογή [Node.js] και ανάπτυξη [Azure εφαρμογής υπηρεσίας] από ένα περιβάλλον γραμμής εντολών, όπως cmd.exe ή πάρτι. Μπορείτε να ακολουθήσετε τις οδηγίες σε αυτό το πρόγραμμα εκμάθησης σε κανένα λειτουργικό σύστημα που μπορεί να εκτελέσει το Node.js.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- [Node.js]
- [Bower]
- [Yeoman]
- [Git]
- [Azure CLI]
- Ένας λογαριασμός Microsoft Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να [εγγραφείτε για μια δωρεάν δοκιμαστική έκδοση] ή να [ενεργοποιήσετε το Visual Studio πλεονεκτήματα συνδρομητών].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Δημιουργία και ανάπτυξη απλό Node.js εφαρμογής web

1. Ανοίξτε το terminal γραμμής εντολών της επιλογής σας και εγκαταστήστε το [Express γεννήτρια για Yeoman].

        npm install -g generator-express

2. `CD`σε έναν κατάλογο εργασίας και να δημιουργήσετε μια εφαρμογή express χρησιμοποιώντας την ακόλουθη σύνταξη:

        yo express
        
    Επιλέξτε τις ακόλουθες επιλογές όταν σας ζητηθεί:  

    `? Would you like to create a new directory for your project?`**Ναι**  
    `? Enter directory name`**{όνομα_εφαρμογής}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Κανένας**  
    `? Select a database to use:`**Κανένας**  
    `? Select a build tool to use:`**Grunt**

3. `CD`στον ριζικό κατάλογο της τη νέα εφαρμογή σας και να ξεκινήσετε την για να βεβαιωθείτε ότι εκτελείται στο περιβάλλον ανάπτυξής σας:

        npm start

    Στο πρόγραμμα περιήγησης, μεταβείτε στις επιλογές <http://localhost:3000> για να βεβαιωθείτε ότι μπορείτε να δείτε την αρχική σελίδα Express. Μόλις επαληθεύσετε την εφαρμογή εκτελείται σωστά, χρησιμοποιήστε `Ctrl-C` για να διακόψετε την.
    
1. Αλλαγή σε κατάσταση λειτουργίας ASM και συνδεθείτε στο Azure (χρειαστεί [Azure CLI](#prereq)):

        azure config mode asm
        azure login

    Ακολουθήστε το προτροπή για να συνεχίσετε τη σύνδεση σε ένα πρόγραμμα περιήγησης με ένα λογαριασμό Microsoft που διαθέτει τη συνδρομή σας στο Azure.

2. Βεβαιωθείτε ότι είστε ακόμη στο ριζικό κατάλογο της εφαρμογής σας και κατόπιν δημιουργήσετε τον πόρο εφαρμογή εφαρμογής υπηρεσίας στο Azure με εφαρμογή μοναδικό όνομα με την επόμενη εντολή. Για παράδειγμα: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Ακολουθήστε το μήνυμα για να επιλέξετε μια περιοχή Azure για ανάπτυξη. Εάν έχετε ποτέ ορίζετε Git/FTP ανάπτυξης διαπιστευτήρια για τη συνδρομή σας Azure, επίσης θα σας ζητηθεί να τα δημιουργήσετε.

3. Ανοίξτε το αρχείο./config/config.js από το ριζικό κατάλογο της εφαρμογής σας και αλλάξτε τη θύρα παραγωγής να `process.env.port`; σας `production` ιδιότητα στο το `config` αντικείμενο θα πρέπει να μοιάζει με το ακόλουθο παράδειγμα:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Αυτό σας επιτρέπει να απαντούν σε web προσκλήσεις σε η προεπιλεγμένη θύρα iisnode που παρακολουθεί την εφαρμογή σας Node.js.
    
4. Ανοίξτε./package.json και προσθέστε το `engines` ιδιοτήτων για να [καθορίσετε την επιθυμητή έκδοση Node.js](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Αποθηκεύστε τις αλλαγές σας και, στη συνέχεια, χρησιμοποιήστε git για να αναπτύξετε την εφαρμογή σας σε Azure:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Η γεννήτρια Express παρέχει ήδη ένα αρχείο .gitignore, επομένως το `git push` δεν κατανάλωση εύρους ζώνης που προσπαθείτε να αποστείλετε το node_modules / καταλόγου.

5. Τέλος, εκκινήστε την εφαρμογή σας live Azure στο πρόγραμμα περιήγησης:

        azure site browse

    Τώρα θα πρέπει να βλέπετε την εφαρμογή web της Node.js εκτελείται live στο Azure εφαρμογής υπηρεσίας.
    
    ![Παράδειγμα της περιήγησης σε εφαρμογής.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Ενημερώστε την εφαρμογή web της Node.js

Για να κάνετε ενημερώσεις σε εφαρμογή web Node.js εκτελείται στο εφαρμογής υπηρεσίας, απλώς εκτέλεση `git add`, `git commit`, και `git push` όπως κάνατε όταν έχετε αναπτύξει πρώτα την εφαρμογή web της.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Τρόπος εφαρμογής υπηρεσίας ανάπτυξη της εφαρμογής σας Node.js

Azure εφαρμογής υπηρεσίας χρησιμοποιεί [iisnode] για να εκτελέσετε Node.js εφαρμογές. Το CLI Azure και ο μηχανισμός Kudu (ανάπτυξη Git) συνεργαστούν για να σας δώσει μια εμπειρία βελτιωμένο κατά την ανάπτυξη και ανάπτυξη εφαρμογών Node.js από τη γραμμή εντολών. 

- `azure site create --git`αναγνωρίζει το κοινό μοτίβο Node.js server.js ή app.js και δημιουργεί μια iisnode.yml του ριζικού καταλόγου. Μπορείτε να χρησιμοποιήσετε αυτό το αρχείο για να προσαρμόσετε iisnode.
- Στο `git push azure master`, Kudu αυτοματοποιεί τις ακόλουθες εργασίες ανάπτυξη:

    - Εάν package.json είναι ο ριζικός κατάλογος αποθετήριο, εκτελέστε `npm install --production`.
    - Μπορείτε να δημιουργείτε μια Web.config για iisnode που οδηγεί στα δέσμη ενεργειών σας έναρξης σε package.json (π.χ., server.js ή app.js).
    - Προσαρμογή Web.config για να προετοιμάσετε την εφαρμογή σας για τον εντοπισμό σφαλμάτων με κόμβο επιθεώρηση.
    
## <a name="use-a-nodejs-framework"></a>Χρησιμοποιήστε ένα πλαίσιο Node.js

Εάν χρησιμοποιείτε ένα δημοφιλείς πλαίσιο Node.js, όπως [Sails.js] [ SAILSJS] ή [MEAN.js] [ MEANJS] για την ανάπτυξη εφαρμογών, μπορείτε να αναπτύξετε εκείνων που εφαρμογής υπηρεσίας. Δημοφιλείς Node.js πλαίσια έχουν τους συγκεκριμένες quirks και τις εξαρτήσεις τους πακέτου διατήρηση γρήγορα ενημερώνονται. Ωστόσο, εφαρμογής υπηρεσίας κάνει τα αρχεία καταγραφής stdout και stderr διαθέσιμη, ώστε να μπορείτε να γνωρίζετε ακριβώς τι συμβαίνει με την εφαρμογή σας και να κάνετε αλλαγές αντίστοιχα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [λήψη stdout και stderr αρχεία καταγραφής από iisnode](#iisnodelog).

Τα παρακάτω προγράμματα εκμάθησης θα σας δείξει πώς να εργάζεστε με ένα συγκεκριμένο πλαίσιο στην εφαρμογή υπηρεσίας:

- [Ανάπτυξη εφαρμογής web Sails.js να Azure εφαρμογής υπηρεσίας]
- [Δημιουργία μιας εφαρμογής συνομιλίας Node.js με Socket.IO στο Azure εφαρμογής υπηρεσίας]
- [Πώς μπορείτε να χρησιμοποιήσετε io.js με Azure εφαρμογής υπηρεσίας Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Χρήση ενός συγκεκριμένου μηχανισμού Node.js

Το τυπικό ροής εργασίας, μπορείτε να καταλάβετε εφαρμογής υπηρεσίας για τη χρήση ενός συγκεκριμένου μηχανισμού Node.js όπως θα κάνατε κανονικά στο package.json.
Για παράδειγμα:

    "engines": {
        "node": "6.6.0"
    }, 

Ο μηχανισμός ανάπτυξης Kudu καθορίζει ποιες μηχανισμός Node.js για χρήση με την εξής σειρά:

- Πρώτα, κοιτάξτε iisnode.yml για να δείτε εάν `nodeProcessCommandLine` έχει καθοριστεί. Εάν επιλέξετε Ναι, στη συνέχεια, χρησιμοποιήστε που.
- Στη συνέχεια, κοιτάξτε package.json για να δείτε εάν `"node": "..."` καθορίζεται στο το `engines` αντικειμένου. Εάν επιλέξετε Ναι, στη συνέχεια, χρησιμοποιήστε που.
- Επιλέξτε μια προεπιλεγμένη έκδοση Node.js από προεπιλογή.

>[AZURE.NOTE] Καλό είναι ότι μπορείτε να ορίσετε ρητά το μηχανισμό Node.js που θέλετε. Να αλλάξετε την προεπιλεγμένη έκδοση Node.js και ενδέχεται να λάβετε σφάλματα στην εφαρμογή Azure web επειδή στην προεπιλεγμένη έκδοση Node.js δεν είναι κατάλληλη για την εφαρμογή σας.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Λήψη αρχείων καταγραφής stdout και stderr από iisnode

Για να διαβάσετε τα αρχεία καταγραφής iisnode, ακολουθήστε τα παρακάτω βήματα.

> [AZURE.NOTE] Αφού ολοκληρώσετε αυτά τα βήματα, τα αρχεία καταγραφής ενδέχεται να μην υπάρχει μέχρι να προκύψει σφάλμα.

1. Ανοίξτε το αρχείο iisnode.yml που παρέχει το Azure CLI.

2. Ρυθμίστε τις παραμέτρους δύο παρακάτω: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Μαζί, λένε iisnode στο εφαρμογής υπηρεσίας για να τοποθετήσετε τα αποτελέσματά stdout και stderror στο το D:\home\site\wwwroot\**iisnode** καταλόγου.

3. Αποθηκεύστε τις αλλαγές σας και, στη συνέχεια, προωθήσετε τις αλλαγές σας σε Azure με τις παρακάτω εντολές Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    τώρα έχει ρυθμιστεί iisnode. Τα επόμενα βήματα που δείχνουν τον τρόπο για να αποκτήσετε πρόσβαση σε αυτά τα αρχεία καταγραφής.
     
4. Στο πρόγραμμα περιήγησης, έχετε πρόσβαση στην κονσόλα εντοπισμού σφαλμάτων Kudu για την εφαρμογή σας, που βρίσκεται στο:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Αυτή η διεύθυνση URL που διαφέρει από τη διεύθυνση URL της εφαρμογής web με την προσθήκη "*.scm.*" Για να το όνομα DNS. Εάν παραλειφθεί το όρισμα που εκτός από τη διεύθυνση URL, θα λάβετε ένα σφάλμα 404.

5. Μεταβείτε στο D:\home\site\wwwroot\iisnode

    ![Περιήγηση στη θέση των αρχείων καταγραφής iisnode.][iislog-kudu-console-find]

6. Κάντε κλικ στο εικονίδιο **επεξεργασίας** για το αρχείο καταγραφής που θέλετε να διαβάσετε. Μπορείτε επίσης να επιλέξετε **λήψη** ή **Διαγραφή** εάν θέλετε.

    ![Κατά το άνοιγμα ενός αρχείου καταγραφής iisnode.][iislog-kudu-console-open]

    Τώρα μπορείτε να δείτε το αρχείο καταγραφής για να σας βοηθήσουν να εντοπίσετε σφάλματα την ανάπτυξη της εφαρμογής υπηρεσίας.
    
    ![Εξέταση ένα αρχείο καταγραφής iisnode.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Εντοπισμός σφαλμάτων την εφαρμογή σας με επιθεώρηση κόμβου

Εάν χρησιμοποιείτε κόμβο επιθεώρηση για τον εντοπισμό σφαλμάτων σε εφαρμογές Node.js σας, μπορείτε να το χρησιμοποιήσετε για την εφαρμογή σας live εφαρμογής υπηρεσίας. Επιθεώρηση κόμβου είναι προεγκατεστημένη στην εγκατάσταση iisnode για την εφαρμογή υπηρεσίας. Και αν αναπτύξετε μέσω Git, το αρχείο Web.config που δημιουργείται αυτόματα από Kudu περιέχει ήδη όλες τις παραμέτρους που χρειάζεστε για να ενεργοποιήσετε την επιθεώρηση κόμβο.

Για να ενεργοποιήσετε τον κόμβο επιθεώρηση, ακολουθήστε τα παρακάτω βήματα:

1. Άνοιγμα iisnode.yml στο τη ριζική αποθετήριο δεδομένων και καθορίστε τις ακόλουθες παραμέτρους: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Αποθηκεύστε τις αλλαγές σας και, στη συνέχεια, προωθήσετε τις αλλαγές σας σε Azure με τις παρακάτω εντολές Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Τώρα, απλώς μεταβείτε αρχείο Έναρξη της εφαρμογής σας, όπως καθορίζονται από τη δέσμη ενεργειών έναρξης στο package.json σας, με/Debug προστεθεί στη διεύθυνση URL. Για παράδειγμα,

        http://{appname}.azurewebsites.net/server.js/debug
    
    Ή,
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Περισσότεροι πόροι

- [Καθορισμός μιας έκδοσης Node.js σε μια εφαρμογή του Azure](../nodejs-specify-node-version-azure-apps.md)
- [Βέλτιστες πρακτικές και Οδηγός αντιμετώπισης προβλημάτων για τις εφαρμογές Node.js στο Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Πώς μπορείτε να εντοπίσετε σφάλματα σε μια εφαρμογή web Node.js στο Azure εφαρμογής υπηρεσίας](web-sites-nodejs-debug.md)
- [Χρήση Node.js λειτουργικές μονάδες με εφαρμογές του Azure](../nodejs-use-node-modules-azure-apps.md)
- [Εφαρμογές Web Azure εφαρμογής υπηρεσίας: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Κέντρο για προγραμματιστές του node.js](/develop/nodejs/)
- [Γρήγορα αποτελέσματα με το web apps στο Azure εφαρμογής υπηρεσίας](app-service-web-get-started.md)
- [Εξερεύνηση της κονσόλας Super μυστικού Kudu εντοπισμού σφαλμάτων]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure εφαρμογής υπηρεσίας]: ../app-service/app-service-value-prop-what-is.md
[Ενεργοποίηση των πλεονεκτημάτων της συνδρομής σας Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Δημιουργία μιας εφαρμογής συνομιλίας Node.js με Socket.IO στο Azure εφαρμογής υπηρεσίας]: ./web-sites-nodejs-chat-app-socketio.md
[Ανάπτυξη εφαρμογής web Sails.js να Azure εφαρμογής υπηρεσίας]: ./app-service-web-nodejs-sails.md
[Εξερεύνηση της κονσόλας Super μυστικού Kudu εντοπισμού σφαλμάτων]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Express γεννήτρια για Yeoman]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Πώς μπορείτε να χρησιμοποιήσετε io.js με Azure εφαρμογής υπηρεσίας Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[εγγραφείτε για μια δωρεάν δοκιμαστική έκδοση]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png

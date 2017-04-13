<properties 
    pageTitle="Εφαρμογή node.js χρησιμοποιώντας Socket.io | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε socket.io σε μια εφαρμογή node.js που φιλοξενούνται στο Azure." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Δημιουργία μιας εφαρμογής Node.js συνομιλίας με Socket.IO σε μια υπηρεσία Azure Cloud

Socket.IO παρέχει επικοινωνία πραγματικού χρόνου μεταξύ μεταξύ του διακομιστή node.js και προγράμματα-πελάτες. Αυτό το πρόγραμμα εκμάθησης θα σας καθοδηγήσει φιλοξενίας υποδοχή. Εισόδου/ΕΞΌΔΟΥ σύμφωνα με εφαρμογή Azure για συνομιλία. Για περισσότερες πληροφορίες σχετικά με Socket.IO, ανατρέξτε στο θέμα <http://socket.io/>.

Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη είναι κάτω από:

![Ένα παράθυρο προγράμματος περιήγησης που εμφανίζει την υπηρεσία που φιλοξενούνται στο Azure][completed-app]  

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Βεβαιωθείτε ότι έχουν εγκατασταθεί τα ακόλουθα προϊόντα και εκδόσεις για να ολοκληρωθεί με επιτυχία το παράδειγμα σε αυτό το άρθρο:

* Εγκατάσταση του [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* Εγκατάσταση [Node.js](https://nodejs.org/download/)
* Εγκατάσταση [Python έκδοση 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Δημιουργία έργου υπηρεσίας Cloud

Ακολουθήστε τα παρακάτω βήματα δημιουργήστε το έργο υπηρεσία cloud που θα φιλοξενήσει την εφαρμογή Socket.IO.

1. Από το **Μενού Έναρξη** ή η **Οθόνη έναρξης**, αναζήτηση για το **Windows PowerShell**. Τέλος, κάντε δεξί κλικ **Του Windows PowerShell** και επιλέξτε **Εκτέλεση ως διαχειριστής**.

    ![Εικονίδιο Azure PowerShell][powershell-menu]

2. Δημιουργήστε έναν κατάλογο που ονομάζεται **c:\\κόμβο**. 
 
        PS C:\> md node

3. Αλλαγή σε καταλόγους για να το **c:\\κόμβου** καταλόγου
 
        PS C:\> cd node

4. Εισαγάγετε τις παρακάτω εντολές για να δημιουργήσετε μια νέα λύση με το όνομα **chatapp** και ένα ρόλο εργασίας με το όνομα **WorkerRole1**:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Θα δείτε την ακόλουθη απόκριση:

    ![Το αποτέλεσμα του νέου azureservice και προσθήκη azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Κάντε λήψη του παραδείγματος συνομιλίας

Για αυτό το έργο, θα χρησιμοποιήσουμε το παράδειγμα συνομιλίας από το [αποθετήριο Socket.IO GitHub]. Ακολουθήστε τα παρακάτω βήματα για να κάνετε λήψη του παραδείγματος και να την προσθέσετε στο έργο που δημιουργήσατε προηγουμένως.

1.  Δημιουργία τοπικού αντιγράφου του αποθετηρίου, χρησιμοποιώντας το κουμπί **αντιγραφής** . Μπορείτε επίσης να χρησιμοποιήσετε το κουμπί **ZIP** για να κάνετε λήψη του έργου.

    ![Προβολή https://github.com/LearnBoost/socket.io/tree/master/examples/chat, με επισήμανση στο εικονίδιο λήψης του ZIP παράθυρο του προγράμματος περιήγησης][chat-example-view]

3.  Να περιηγούνται στη δομή καταλόγου του τοπικού αποθετηρίου μέχρι να φτάσετε σε το **Παραδείγματα\\συνομιλίας** καταλόγου. Αντιγράψτε τα περιεχόμενα αυτού του καταλόγου για το **C:\\κόμβου\\chatapp\\WorkerRole1** καταλόγου που δημιουργήσατε νωρίτερα.

    ![Εξερεύνηση, εμφάνιση των περιεχομένων από τα παραδείγματα\\καταλόγου συνομιλίας που έχουν εξαχθεί από το αρχείο αρχειοθέτησης][chat-contents]

    Τα στοιχεία με επισήμανση στο στιγμιότυπο οθόνης παραπάνω είναι τα αρχεία που αντιγράψατε από το **Παραδείγματα\\συνομιλίας** καταλόγου

4.  Στο το **C:\\κόμβου\\chatapp\\WorkerRole1** καταλόγου, διαγράψτε το αρχείο **server.js** και, στη συνέχεια, να μετονομάσετε το αρχείο **app.js** σε **server.js**. Αυτό καταργεί το προεπιλεγμένο αρχείο **server.js** που δημιουργήσατε προηγουμένως με το cmdlet **Προσθήκη AzureNodeWorkerRole** και αντικαθιστά με το αρχείο της εφαρμογής από το παράδειγμα συνομιλίας.

### <a name="modify-serverjs-and-install-modules"></a>Τροποποίηση Server.js και εγκατάσταση λειτουργικές μονάδες

Πριν να δοκιμάσετε την εφαρμογή στο το Azure προσομοίωσης, πρέπει να κάνουμε ορισμένες μικρές τροποποιήσεις. Ακολουθήστε τα παρακάτω βήματα για να το αρχείο server.js:

1.  Ανοίξτε το αρχείο **server.js** στο Visual Studio ή οποιοδήποτε πρόγραμμα επεξεργασίας κειμένου.

2.  Βρείτε την ενότητα **εξαρτήσεις λειτουργικής μονάδας** στην αρχή του server.js και αλλάξτε τη γραμμή που περιέχει **sio = require('.. //.. lib//Socket.IO')** να **sio = require('socket.io')** όπως φαίνεται παρακάτω:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Για να βεβαιωθείτε ότι η εφαρμογή ακρόασης για τη σωστή θύρα, ανοίξτε server.js στο Σημειωματάριο ή πρόγραμμα επεξεργασίας Αγαπημένα και, στη συνέχεια, αλλάξτε την ακόλουθη γραμμή, αντικαθιστώντας **3000** με **process.env.port** , όπως φαίνεται παρακάτω:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Αφού αποθηκεύσετε τις αλλαγές σε **server.js**, χρησιμοποιήστε τα ακόλουθα βήματα για να εγκαταστήσετε το απαιτούμενο λειτουργικές μονάδες και, στη συνέχεια, να δοκιμάσετε την εφαρμογή σε το Azure προσομοίωσης:

1.  Χρήση του **Azure PowerShell**, αλλάξτε τον κατάλογο για να το **C:\\κόμβου\\chatapp\\WorkerRole1** καταλόγου και χρησιμοποιήστε την ακόλουθη εντολή για να εγκαταστήσετε τις λειτουργικές μονάδες που απαιτούνται από αυτήν την εφαρμογή:

        PS C:\node\chatapp\WorkerRole1> npm install

    Αυτό θα εγκαταστήσει τις λειτουργικές μονάδες που αναφέρονται στο αρχείο package.json. Μόλις ολοκληρωθεί η εντολή, θα πρέπει να βλέπετε εξόδου παρόμοια με τα εξής:

    ![Το αποτέλεσμα του npm εντολής εγκατάστασης][The-output-of-the-npm-install-command]

4.  Επειδή αυτό το παράδειγμα είχε αρχικά ένα τμήμα του αποθετηρίου Socket.IO GitHub και άμεση αναφορά στη βιβλιοθήκη Socket.IO σχετική διαδρομή, Socket.IO δεν έγινε αναφορά στο αρχείο package.json, ώστε να σας πρέπει να το εγκαταστήσετε με την έκδοση την ακόλουθη εντολή:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Δοκιμή και ανάπτυξη

1.  Εκκινήστε το προσομοίωσης με την έκδοση την ακόλουθη εντολή:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Ανοίξτε ένα πρόγραμμα περιήγησης και μεταβείτε σε **http://127.0.0.1**.

3.  Όταν ανοίξει το παράθυρο του προγράμματος περιήγησης, πληκτρολογήστε ένα ψευδώνυμο και, στη συνέχεια, πληκτρολογήστε αντιστοιχία.
    Αυτό θα όλα μπορείτε να δημοσιεύσετε μηνύματα ως ένα συγκεκριμένο ψευδώνυμο. Για να δοκιμάσετε τη λειτουργικότητα πολλών χρηστών, ανοίξτε το πρόσθετο προγράμματος περιήγησης των windows χρησιμοποιώντας την ίδια διεύθυνση URL και εισαγάγετε διαφορετικό ψευδώνυμα.

    ![Εμφάνιση μηνυμάτων συνομιλίας από Χρήστη1 και το Χρήστη2 δύο παράθυρα του προγράμματος περιήγησης](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Μετά τη δοκιμή της εφαρμογής, διακόψετε το προσομοίωσης με την έκδοση την ακόλουθη εντολή:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Για να αναπτύξετε την εφαρμογή για να Azure, χρησιμοποιήστε το cmdlet **AzureServiceProject δημοσίευση** . Για παράδειγμα:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Φροντίστε να χρησιμοποιήσετε ένα μοναδικό όνομα, διαφορετικά η διαδικασία δημοσίευσης θα αποτύχει. Μετά την ολοκλήρωση της ανάπτυξης, το πρόγραμμα περιήγησης θα ανοίξετε και να μεταβείτε στην υπηρεσία ανεπτυγμένος.
    > 
    > Εάν λάβετε ένα σφάλμα που δηλώνει ότι δεν υπάρχει το όνομα της συνδρομής που παρέχονται στο προφίλ που έχουν εισαχθεί δημοσίευση, πρέπει να κάνετε λήψη και εισαγάγετε το προφίλ δημοσίευσης για τη συνδρομή σας πριν από την ανάπτυξη στον Azure. Ανατρέξτε στην ενότητα **για την ανάπτυξη της εφαρμογής σε Azure** του [Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Ένα παράθυρο προγράμματος περιήγησης που εμφανίζει την υπηρεσία που φιλοξενούνται στο Azure][completed-app]

    > [AZURE.NOTE] Εάν λάβετε ένα σφάλμα που δηλώνει ότι δεν υπάρχει το όνομα της συνδρομής που παρέχονται στο προφίλ που έχουν εισαχθεί δημοσίευση, πρέπει να κάνετε λήψη και εισαγάγετε το προφίλ δημοσίευσης για τη συνδρομή σας πριν από την ανάπτυξη στον Azure. Ανατρέξτε στην ενότητα **για την ανάπτυξη της εφαρμογής σε Azure** του [Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

Η εφαρμογή σας εκτελείται τώρα στον Azure και μπορούν να μεταβιβάσουν μηνύματα συνομιλίας ανάμεσα σε διαφορετικούς υπολογιστές-πελάτες που χρησιμοποιούν Socket.IO.

> [AZURE.NOTE] Για λόγους ευκολίας, αυτό το δείγμα περιορίζεται σε συνομιλία μεταξύ τους χρήστες που συνδέονται με την ίδια περίοδο λειτουργίας. Αυτό σημαίνει ότι εάν η υπηρεσία cloud δημιουργεί δύο παρουσίες ρόλο εργασίας, οι χρήστες μόνο θα μπορούν να συνομιλείτε με άλλους συνδεδεμένο με την ίδια παρουσία ρόλο εργασίας. Για να κλιμακωθεί της εφαρμογής για να εργαστείτε με πολλές παρουσίες ρόλο, μπορείτε να χρησιμοποιήσετε μια τεχνολογία όπως Bus υπηρεσίας για να κάνετε κοινή χρήση της κατάστασης store Socket.IO όλων των παρουσιών. Για παραδείγματα, ανατρέξτε στο θέμα τα δείγματα χρήση ουρές Bus υπηρεσία και τα θέματα στο [SDK Azure για το αποθετήριο Node.js GitHub](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης μάθατε πώς μπορείτε να δημιουργήσετε μια εφαρμογή βασικές συνομιλίας που φιλοξενούνται σε μια υπηρεσία Cloud Azure. Για να μάθετε πώς μπορείτε να φιλοξενήσετε αυτής της εφαρμογής σε μια τοποθεσία Web του Azure, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής Node.js συνομιλείτε με Socket.IO στην τοποθεσία Web Azure][chatwebsite].

Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές του Node.js](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub αποθετήριο δεδομένων]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 

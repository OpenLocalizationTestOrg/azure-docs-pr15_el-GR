<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από Node.js | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ουράς Azure για να δημιουργήσετε και να διαγράψετε ουρές, εισαγωγή, λήψη και να διαγράψετε μηνύματα. Δείγματα γραμμένο σε Node.js."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από Node.js

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός δείχνει πώς μπορείτε να εκτελέσετε συνηθισμένα σενάρια με την υπηρεσία Microsoft Azure ουρά. Τα δείγματα γράφονται με χρήση του API Node.js. Τα σενάρια καλύπτεται περιλαμβάνουν **εισαγωγής**, **παρατήρηση**, **λήψη**και **Διαγραφή** ουρά μηνυμάτων, καθώς και **τη δημιουργία και διαγραφή ουρές**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Δημιουργία μιας εφαρμογής Node.js

Δημιουργήστε μια κενή εφαρμογή Node.js. Για τη δημιουργία μιας εφαρμογής Node.js οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εφαρμογή web Node.js στο Azure εφαρμογής υπηρεσίας], [Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure] με τη χρήση του Windows PowerShell ή [Δημιουργήστε και αναπτύξτε μια εφαρμογή web Node.js να Azure χρήση πίνακα Web].

## <a name="configure-your-application-to-access-storage"></a>Ρύθμιση παραμέτρων την εφαρμογή με το χώρο αποθήκευσης της Access

Για να χρησιμοποιήσετε Azure χώρου αποθήκευσης, χρειάζεστε το Azure SDK χώρου αποθήκευσης για Node.js, η οποία περιλαμβάνει ένα σύνολο βιβλιοθηκών ευκολία που επικοινωνία με τις υπηρεσίες ΥΠΌΛΟΙΠΟ του χώρου αποθήκευσης.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Χρήση διαχείρισης πακέτου κόμβου (NPM) για να αποκτήσετε το πακέτο

1.  Χρησιμοποιήστε ένα περιβάλλον γραμμής εντολών όπως **PowerShell** (Windows), **Terminal** (Mac) ή το **πάρτι** (Unix), μεταβείτε στο φάκελο όπου δημιουργήσατε την εφαρμογή του δείγματος.

2.  Πληκτρολογήστε **npm εγκατάσταση αποθήκευσης azure** στο παράθυρο εντολών. Έξοδος από την εντολή είναι παρόμοιο με το παρακάτω παράδειγμα.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Μπορείτε να εκτελέσετε με μη αυτόματο τρόπο την εντολή **ls** για να επιβεβαιώσετε ότι ένα **κόμβου\_λειτουργικές μονάδες** που δημιουργήθηκε το φάκελο. Μέσα σε αυτόν το φάκελο θα βρείτε το πακέτο **αποθήκευσης azure** , η οποία περιέχει τις βιβλιοθήκες που πρέπει να έχουν πρόσβαση χώρου αποθήκευσης.

### <a name="import-the-package"></a>Εισαγωγή του πακέτου

Χρησιμοποιείτε το Σημειωματάριο ή κάποιο άλλο πρόγραμμα επεξεργασίας κειμένου, προσθέστε τα ακόλουθα στο επάνω μέρος του αρχείου **server.js** της εφαρμογής όπου σκοπεύετε να χρησιμοποιήσετε αποθήκευσης:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Ρυθμίσετε μια σύνδεση Azure χώρου αποθήκευσης

Τη λειτουργική μονάδα azure θα διαβάσει τις μεταβλητές περιβάλλοντος AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ΛΟΓΑΡΙΑΣΜΟΎ και AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ACCESS\_ΚΛΕΙΔΊ ή AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ΣΎΝΔΕΣΗΣ\_ΣΥΜΒΟΛΟΣΕΙΡΆΣ για τις πληροφορίες που απαιτούνται για να συνδεθείτε στο λογαριασμό σας Azure χώρου αποθήκευσης. Εάν δεν έχουν οριστεί αυτές τις μεταβλητές περιβάλλοντος, πρέπει να καθορίσετε τις πληροφορίες λογαριασμού κατά την κλήση **createQueueService**.

Ένα παράδειγμα για τη ρύθμιση των μεταβλητών περιβάλλοντος στην [Πύλη του Azure](https://portal.azure.com) για μια τοποθεσία Web του Azure, ανατρέξτε στο θέμα [Node.js εφαρμογής web με την υπηρεσία πίνακα Azure].

## <a name="how-to-create-a-queue"></a>Τρόπος: Δημιουργία ουράς

Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **QueueService** , το οποίο σας επιτρέπει να εργαστείτε με ουρές.

    var queueSvc = azure.createQueueService();

Χρησιμοποιήστε τη μέθοδο **createQueueIfNotExists** , το οποίο επιστρέφει την καθορισμένη ουρά, εάν υπάρχει ήδη ή δημιουργεί μια νέα ουρά με το καθορισμένο όνομα, εάν δεν υπάρχει ήδη.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Εάν η ουρά δημιουργείται, `result.created` είναι αληθής. Εάν υπάρχει ουρά, `result.created` είναι ψευδής.

### <a name="filters"></a>Φίλτρα

Προαιρετικό λειτουργίες φιλτραρίσματος μπορούν να εφαρμοστούν σε διάφορες λειτουργίες που εκτελούνται με χρήση **QueueService**. Φιλτράρισμα λειτουργίες μπορούν να περιλαμβάνουν καταγραφής, αυτόματη επανάληψη, κ.λπ. Φίλτρα είναι αντικείμενα τα οποία υλοποίηση της μεθόδου με την υπογραφή:

    function handle (requestOptions, next)

Μετά την εκτέλεση τις προ-επεξεργασία αρχείου σχετικά με τις επιλογές αίτηση, τη μέθοδο πρέπει να καλέσετε "Επόμενο" που περνά μέσα επιστροφή κλήσης με την εξής υπογραφή:

    function (returnObject, finalCallback, next)

Με αυτήν την επιστροφή κλήσης, και μετά την επεξεργασία του returnObject (απάντηση από την αίτηση στο διακομιστή), η επιστροφή κλήσης πρέπει για κλήση Επόμενο, εάν υπάρχει να συνεχίσετε την επεξεργασία τα άλλα φίλτρα ή απλώς κλήση διαφορετικά finalCallback για να τερματίσετε την υπηρεσία κλήσης προς τα επάνω.

Με το Azure SDK για Node.js, **ExponentialRetryPolicyFilter** και **LinearRetryPolicyFilter**περιλαμβάνονται δύο φίλτρα που υλοποίηση της λογικής "Επανάληψη". Το ακόλουθο παράδειγμα δημιουργεί ένα αντικείμενο **QueueService** που χρησιμοποιεί το **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Τρόπος: Εισαγάγετε ένα μήνυμα σε μια ουρά

Για να εισαγάγετε ένα μήνυμα σε μια ουρά, χρησιμοποιήστε τη μέθοδο **createMessage** για να δημιουργήσετε ένα νέο μήνυμα και να την προσθέσετε στην ουρά.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Τρόπος: Ρίξτε μια ματιά σε στο επόμενο μήνυμα

Μπορείτε να Ρίξτε μια ματιά σε το μήνυμα στο μπροστινό μέρος μια ουρά χωρίς να την καταργήσετε από την ουρά καλώντας τη μέθοδο **peekMessages** . Από προεπιλογή, **peekMessages** συνόψεις σε ένα μήνυμα.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

Το `result` περιέχει το μήνυμα.

> [AZURE.NOTE] Χρήση **peekMessages** όταν υπάρχουν χωρίς μηνύματα στην ουρά θα επιστρέψουν σφάλμα, ωστόσο χωρίς μηνύματα θα επιστραφεί.

## <a name="how-to-dequeue-the-next-message"></a>Τρόπος: Απομάκρυνση από την ουρά του επόμενου μηνύματος

Επεξεργασία ενός μηνύματος είναι μια διαδικασία δύο σταδίων:

1. Απομάκρυνση από την ουρά του μηνύματος.

2. Διαγραφή του μηνύματος.

Για να κατάργησης ουράς ένα μήνυμα, χρησιμοποιήστε **getMessages**. Έτσι αόρατο στην ουρά, τα μηνύματα, ώστε να χωρίς άλλα προγράμματα-πελάτες μπορεί να επεξεργαστεί τους. Όταν η εφαρμογή σας έχει υποβληθεί σε επεξεργασία ένα μήνυμα, κλήση **deleteMessage** για να τον διαγράψετε από την ουρά. Το παρακάτω παράδειγμα λαμβάνει ένα μήνυμα και, στη συνέχεια, διαγράφει το:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Από προεπιλογή, ένα μήνυμα είναι ορατή μόνο για 30 δευτερόλεπτα, μετά από το οποίο είναι ορατό σε άλλα προγράμματα-πελάτες. Μπορείτε να καθορίσετε μια διαφορετική τιμή με τη χρήση `options.visibilityTimeout` με **getMessages**.

> [AZURE.NOTE]
> Χρήση **getMessages** όταν υπάρχουν χωρίς μηνύματα στην ουρά θα επιστρέψουν σφάλμα, ωστόσο χωρίς μηνύματα θα επιστραφεί.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Τρόπος: Αλλαγή των περιεχομένων ένα μήνυμα σε ουρά

Μπορείτε να αλλάξετε τα περιεχόμενα ενός μηνύματος στην ίδια θέση στην ουρά με χρήση **updateMessage**. Το παρακάτω παράδειγμα ενημερώνει το κείμενο ενός μηνύματος:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Τρόπος: Πρόσθετες επιλογές για κατάργησης μηνυμάτων

Υπάρχουν δύο τρόποι για να προσαρμόσετε ανάκτηση μηνύματος από μια ουρά:

* `options.numOfMessages`-Ανάκτηση μια δέσμη μηνυμάτων (έως 32).
* `options.visibilityTimeout`-Ορίστε ένα χρονικό όριο invisibility μεγαλύτερη ή μικρότερη.

Το παρακάτω παράδειγμα χρησιμοποιεί τη μέθοδο **getMessages** για να λάβετε μηνύματα 15 σε μία κλήση. Στη συνέχεια, επεξεργάζεται κάθε μηνύματος ηλεκτρονικού ταχυδρομείου χρησιμοποιώντας μια για επανάληψη. Επίσης, ορίζει το χρονικό όριο invisibility έως πέντε λεπτά για όλα τα μηνύματα που επιστρέφονται από αυτήν τη μέθοδο.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Τον τρόπο: Το μήκος ουράς γρήγορα

Το **getQueueMetadata** επιστρέφει μετα-δεδομένα σχετικά με την ουρά, συμπεριλαμβανομένου του αριθμού κατά προσέγγιση του τα μηνύματα που αναμένουν στην ουρά.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Τρόπος: Λίστα ουρές

Για να ανακτήσετε μια λίστα με τις ουρές, χρησιμοποιήστε **listQueuesSegmented**. Για να ανακτήσετε μια λίστα φιλτραρισμένο κατά ένα συγκεκριμένο πρόθεμα, χρησιμοποιήστε **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Εάν δεν μπορεί να επιστραφεί όλες τις ουρές, `result.continuationToken` μπορεί να χρησιμοποιηθεί ως η πρώτη παράμετρος της **listQueuesSegmented** ή η δεύτερη παράμετρος της **listQueuesSegmentedWithPrefix** για να ανακτήσετε περισσότερα αποτελέσματα.

## <a name="how-to-delete-a-queue"></a>Τρόπος: Διαγραφή ουράς

Για να διαγράψετε μια ουρά και όλα τα μηνύματα που περιέχονται σε αυτό, καλέστε τη μέθοδο **deleteQueue** του αντικειμένου ουράς.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Για να καταργήσετε όλα τα μηνύματα από μια ουρά χωρίς να το διαγράψετε, χρησιμοποιήστε **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Πώς μπορείτε να: εργασία με κοινόχρηστη πρόσβαση υπογραφών

Συσχετίσεις κοινόχρηστο Access υπογραφές (Ασφαλείας) είναι ένας ασφαλής τρόπος για την παροχή λεπτομερούς πρόσβασης σε ουρές χωρίς να παρέχετε το όνομα του λογαριασμού χώρου αποθήκευσης ή πλήκτρα. Συσχετίσεις Ασφαλείας συχνά χρησιμοποιούνται για την παροχή περιορισμένη πρόσβαση σε ουρές σας, όπως παρέχει μια εφαρμογή για κινητές συσκευές για να υποβάλετε τα μηνύματα.

Μια αξιόπιστη εφαρμογή, όπως μια υπηρεσία που βασίζεται στο cloud δημιουργεί μια συσχετισμών Ασφαλείας χρησιμοποιώντας το **generateSharedAccessSignature** από το **QueueService**και παρέχει σε μια εφαρμογή του μη αξιόπιστα ή ημι-αξιόπιστη. Για παράδειγμα, μια εφαρμογή για κινητές συσκευές. Τις συσχετίσεις Ασφαλείας δημιουργείται χρησιμοποιώντας μια πολιτική, η οποία περιγράφει τις ημερομηνίες έναρξης και λήξης κατά την οποία είναι έγκυρη τις συσχετίσεις Ασφαλείας, καθώς και το επίπεδο πρόσβασης που έχει εκχωρηθεί στον κάτοχο συσχετισμών Ασφαλείας.

Το παρακάτω παράδειγμα δημιουργεί μια νέα πολιτική κοινόχρηστο πρόσβασης που θα σας επιτρέψει ο κάτοχος συσχετισμών Ασφαλείας για να προσθέσετε τα μηνύματα στην ουρά και λήξη 100 λεπτά μετά την ώρα που έχει δημιουργηθεί.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Σημειώστε ότι οι πληροφορίες του κεντρικού υπολογιστή πρέπει να δοθεί επίσης, όπως απαιτείται όταν ο κάτοχος συσχετισμών Ασφαλείας προσπαθεί να αποκτήσει πρόσβαση στην ουρά.

Στη συνέχεια, την εφαρμογή-πελάτη χρησιμοποιεί τις συσχετίσεις Ασφαλείας με **QueueServiceWithSAS** για την εκτέλεση λειτουργιών σε σχέση με ουρά. Το παρακάτω παράδειγμα συνδέεται στην ουρά και δημιουργεί ένα μήνυμα.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Επειδή το συσχετισμών Ασφαλείας δημιουργήθηκε με την προσθήκη πρόσβαση, εάν η προσπάθεια έγιναν για ανάγνωση, ενημέρωση ή διαγραφή μηνυμάτων, θα επιστραφεί ένα σφάλμα.

### <a name="access-control-lists"></a>Λίστες ελέγχου πρόσβασης

Μπορείτε επίσης να χρησιμοποιήσετε μια λίστα ελέγχου πρόσβασης (ACL) για να ορίσετε την πολιτική πρόσβασης για μια συσχετισμούς Ασφαλείας. Αυτό είναι χρήσιμο εάν θέλετε για να επιτρέψετε σε πολλούς υπολογιστές-πελάτες για να αποκτήσετε πρόσβαση στην ουρά, αλλά παρέχει πολιτικές πρόσβασης διαφορετικό για κάθε πελάτη.

Ένα ACL έχει υλοποιηθεί με χρήση ενός πίνακα της access πολιτικές, με το Αναγνωριστικό που σχετίζονται με κάθε πολιτικής. Το παρακάτω παράδειγμα καθορίζει δύο πολιτικές. ένα για 'user1' και μία για 'user2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Το παρακάτω παράδειγμα λαμβάνει την τρέχουσα ACL για **myqueue**και, στη συνέχεια, προσθέτει τις νέες πολιτικές που χρησιμοποιεί **setQueueAcl**. Αυτή η προσέγγιση σας επιτρέπει:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Όταν έχει οριστεί η ACL, μπορείτε να δημιουργήσετε μια συσχετισμών Ασφαλείας με βάση το Αναγνωριστικό για μια πολιτική. Το παρακάτω παράδειγμα δημιουργεί μια νέα συσχετισμών Ασφαλείας για 'user2':

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του χώρου αποθήκευσης ουρά, ακολουθήστε αυτές τις συνδέσεις για να μάθετε σχετικά με τις πιο σύνθετες εργασίες χώρου αποθήκευσης.

-   Επισκεφθείτε το [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης][].
-   Επισκεφθείτε το αποθετήριο [Azure SDK χώρου αποθήκευσης για κόμβο][] στο GitHub.

  [Azure αποθήκευσης SDK για κόμβου]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Δημιουργία εφαρμογής web Node.js στο Azure εφαρμογής υπηρεσίας]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js εφαρμογής web με την υπηρεσία πίνακα Azure]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/
  [Δημιουργήστε και αναπτύξτε μια εφαρμογή web Node.js στον Azure χρήση πίνακα Web]: ../app-service-web/web-sites-nodejs-use-webmatrix.md

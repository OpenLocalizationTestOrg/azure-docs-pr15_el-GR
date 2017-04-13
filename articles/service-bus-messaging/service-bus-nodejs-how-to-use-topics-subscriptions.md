<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε τα θέματα υπηρεσίας Bus με Node.js | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε θέματα Bus υπηρεσίας και συνδρομές στο Azure από μια εφαρμογή Node.js." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Τον τρόπο χρήσης υπηρεσίας Bus θέματα και συνδρομών

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Αυτός ο οδηγός περιγράφει τον τρόπο χρήσης της υπηρεσίας Bus θέματα και οι συνδρομές από τις εφαρμογές Node.js. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία θέματα και οι συνδρομές**, **τη δημιουργία συνδρομής φίλτρα**, **Αποστολή μηνυμάτων** με ένα θέμα, **λαμβάνετε μηνύματα από μια συνδρομή**και **Διαγραφή θέματα και συνδρομών**. Για περισσότερες πληροφορίες σχετικά με τα θέματα και οι συνδρομές, ανατρέξτε στην ενότητα [επόμενα βήματα](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Δημιουργία μιας εφαρμογής Node.js

Δημιουργήστε μια κενή εφαρμογή Node.js. Για οδηγίες σχετικά με τη δημιουργία μιας εφαρμογής Node.js, ανατρέξτε στο θέμα [Δημιουργία και ανάπτυξη μιας εφαρμογής Node.js σε μια τοποθεσία Web Azure], [Υπηρεσία Cloud Node.js][] με τη χρήση του Windows PowerShell ή τοποθεσία Web με WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να χρησιμοποιήσετε Bus υπηρεσίας

Για να χρησιμοποιήσετε την υπηρεσία Bus, λήψη του πακέτου Node.js Azure. Αυτό το πακέτο περιλαμβάνει ένα σύνολο των βιβλιοθηκών που επικοινωνία με τις υπηρεσίες ΥΠΌΛΟΙΠΑ Bus υπηρεσίας.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Χρήση διαχείρισης πακέτου κόμβου (NPM) για να αποκτήσετε το πακέτο

1.  Χρησιμοποιήστε ένα περιβάλλον γραμμής εντολών όπως **PowerShell** (Windows), **Terminal** (Mac) ή το **πάρτι** (Unix), μεταβείτε στο φάκελο όπου δημιουργήσατε την εφαρμογή του δείγματος.

2.  Πληκτρολογήστε **npm εγκατάσταση azure** στο παράθυρο εντολών, το οποίο θα πρέπει να έχει ως αποτέλεσμα το εξής αποτέλεσμα:

    ```
        azure@0.7.5 node_modules\azure
    ├── dateformat@1.0.2-1.2.3
    ├── xmlbuilder@0.4.2
    ├── node-uuid@1.2.0
    ├── mime@1.2.9
    ├── underscore@1.4.4
    ├── validator@1.1.1
    ├── tunnel@0.0.2
    ├── wns@0.5.3
    ├── xml2js@0.2.7 (sax@0.5.2)
    └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3.  Μπορείτε να εκτελέσετε με μη αυτόματο τρόπο την εντολή **ls** για να επιβεβαιώσετε ότι ένα **κόμβου\_λειτουργικές μονάδες** που δημιουργήθηκε το φάκελο. Μέσα σε αυτόν το φάκελο, βρείτε το πακέτο **azure** , η οποία περιέχει τις βιβλιοθήκες που χρειάζεστε για να αποκτήσετε πρόσβαση σε θέματα Bus υπηρεσίας.

### <a name="import-the-module"></a>Εισαγωγή της λειτουργικής μονάδας

Χρησιμοποιείτε το Σημειωματάριο ή κάποιο άλλο πρόγραμμα επεξεργασίας κειμένου, προσθέστε τα ακόλουθα στο επάνω μέρος του αρχείου **server.js** της εφαρμογής:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Ρύθμιση σύνδεσης Bus υπηρεσίας

Η λειτουργική μονάδα Azure διαβάζει τις μεταβλητές περιβάλλοντος AZURE\_SERVICEBUS\_χώρο ΟΝΟΜΆΤΩΝ και AZURE\_SERVICEBUS\_ACCESS\_ΒΑΣΙΚΈΣ πληροφορίες που απαιτούνται για σύνδεση σε υπηρεσία Bus. Εάν δεν έχουν οριστεί αυτές τις μεταβλητές περιβάλλοντος, πρέπει να καθορίσετε τις πληροφορίες λογαριασμού κατά την κλήση **createServiceBusService**.

Για ένα παράδειγμα της ρύθμισης των μεταβλητών περιβάλλοντος σε ένα αρχείο ρύθμισης παραμέτρων για μια υπηρεσία Cloud Azure, ανατρέξτε στο θέμα [Node.js υπηρεσία Cloud με το χώρο αποθήκευσης][].

Ένα παράδειγμα για τη ρύθμιση των μεταβλητών περιβάλλοντος στην [πύλη του Azure κλασική][] για μια τοποθεσία Web του Azure, ανατρέξτε στο θέμα [Node.js εφαρμογής Web με το χώρο αποθήκευσης][].

## <a name="create-a-topic"></a>Δημιουργία θέματος

Το αντικείμενο **ServiceBusService** σάς επιτρέπει να εργαστείτε με τα θέματα. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **ServiceBusService** . Προσθέστε την κοντά στο επάνω μέρος του αρχείου **server.js** , μετά την πρόταση για να εισαγάγετε τη λειτουργική μονάδα azure:

```
var serviceBusService = azure.createServiceBusService();
```

Καλώντας την **createTopicIfNotExists** στο αντικείμενο **ServiceBusService** , το συγκεκριμένο θέμα θα επιστραφεί (εάν υπάρχει) ή θα δημιουργηθεί ένα νέο θέμα με το καθορισμένο όνομα. Ο ακόλουθος κώδικας χρησιμοποιεί **createTopicIfNotExists** για να δημιουργήσετε ή να συνδεθείτε με το θέμα που ονομάζεται 'MyTopic':

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** υποστηρίζει επίσης πρόσθετες επιλογές, οι οποίες σας επιτρέπουν να Παράκαμψη προεπιλεγμένων ρυθμίσεων θέμα όπως μήνυμα διάρκεια ζωής ή το θέμα μέγιστο μέγεθος. Το παρακάτω παράδειγμα ορίζει το θέμα μέγιστο μέγεθος 5GB με μια διάρκεια ζωής του 1 λεπτού:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Φίλτρα

Προαιρετικό λειτουργίες φιλτραρίσματος μπορούν να εφαρμοστούν σε διάφορες λειτουργίες που εκτελούνται με χρήση **ServiceBusService**. Φιλτράρισμα λειτουργίες μπορούν να περιλαμβάνουν καταγραφής, αυτόματη επανάληψη, κ.λπ. Φίλτρα είναι αντικείμενα τα οποία υλοποίηση της μεθόδου με την υπογραφή:

```
function handle (requestOptions, next)
```

Μετά την εκτέλεση προ-επεξεργασία αρχείου σχετικά με τις επιλογές αίτηση, καλεί τη μέθοδο `next` που περνά μέσα επιστροφή κλήσης με την υπογραφή παρακάτω:

```
function (returnObject, finalCallback, next)
```

Με αυτήν την επιστροφή κλήσης, και μετά την επεξεργασία του **returnObject** (απάντηση από την αίτηση στο διακομιστή), τις ανάγκες επιστροφής κλήσης σε ένα κλήση Επόμενο εάν υπάρχει για συνεχίσετε την επεξεργασία τα άλλα φίλτρα ή απλώς κλήση διαφορετικά **finalCallback** για να τερματίσετε την υπηρεσία κλήσης προς τα επάνω.

Με το Azure SDK για Node.js, **ExponentialRetryPolicyFilter** και **LinearRetryPolicyFilter**περιλαμβάνονται δύο φίλτρα που υλοποίηση της λογικής "Επανάληψη". Το ακόλουθο παράδειγμα δημιουργεί ένα αντικείμενο **ServiceBusService** που χρησιμοποιεί το **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Δημιουργία εγγραφών

Το θέμα συνδρομές δημιουργούνται επίσης με το αντικείμενο **ServiceBusService** . Συνδρομές ονομάζονται και μπορούν να έχουν ένα προαιρετικό φίλτρο που περιορίζει το σύνολο των μηνυμάτων που παραδίδονται σε ουρά εικονικού τη συνδρομή.

> [AZURE.NOTE] Συνδρομές είναι μόνιμες και θα εξακολουθούν να υπάρχουν παρά μόνο αφού είτε ή στο θέμα συσχετίζονται με, διαγράφονται. Εάν η εφαρμογή σας περιέχει λογική για να δημιουργήσετε μια συνδρομή, το πρώτα πρέπει να ελέγξετε αν υπάρχει ήδη τη συνδρομή, χρησιμοποιώντας τη μέθοδο **getSubscription** .

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Δημιουργήστε μια συνδρομή με το φίλτρο προεπιλεγμένη (MatchAll)

Το φίλτρο **MatchAll** είναι το προεπιλεγμένο φίλτρο που χρησιμοποιείται, αν υπάρχει φίλτρο έχει καθοριστεί όταν δημιουργείται μια νέα συνδρομή. Όταν χρησιμοποιείται το φίλτρο **MatchAll** , όλα τα μηνύματα που δημοσιεύονται στο θέμα τοποθετούνται στην ουρά εικονικού τη συνδρομή. Το παρακάτω παράδειγμα δημιουργεί μια συνδρομή με το όνομα 'AllMessages' και χρησιμοποιεί το προεπιλεγμένο φίλτρο **MatchAll** .

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Δημιουργία συνδρομές με τα φίλτρα

Μπορείτε επίσης να δημιουργήσετε τα φίλτρα που σας επιτρέπει να εμβέλειας που μηνύματα που αποστέλλονται σε ένα θέμα θα πρέπει να εμφανίζονται μέσα σε μια συνδρομή στο συγκεκριμένο θέμα.

Η πιο ευέλικτη τύπο φίλτρου που υποστηρίζονται από συνδρομές είναι το **SqlFilter**, που εφαρμόζει ένα υποσύνολο SQL92. Φίλτρα SQL λειτουργούν σχετικά με τις ιδιότητες των μηνυμάτων που δημοσιεύονται στο θέμα. Για περισσότερες λεπτομέρειες σχετικά με τις παραστάσεις που μπορούν να χρησιμοποιηθούν με φίλτρο SQL, εξετάστε το [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] σύνταξη.

Φίλτρα μπορούν να προστεθούν σε μια συνδρομή, χρησιμοποιώντας τη μέθοδο **createRule** του αντικειμένου **ServiceBusService** . Αυτή η μέθοδος σάς επιτρέπει να προσθέσετε νέα φίλτρα σε μια υπάρχουσα συνδρομή.

> [AZURE.NOTE] Επειδή το προεπιλεγμένο φίλτρο εφαρμόζεται αυτόματα σε όλες τις νέες εγγραφές, πρέπει πρώτα να καταργήσετε το προεπιλεγμένο φίλτρο ή το **MatchAll** θα αντικαταστήσει τα άλλα φίλτρα μπορείτε να καθορίσετε. Μπορείτε να καταργήσετε τον προεπιλεγμένο κανόνα, χρησιμοποιώντας τη μέθοδο **η DeleteRule δεν** του αντικειμένου **ServiceBusService** .

Το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα `HighMessages` με **SqlFilter** που επιλέγει μόνο τα μηνύματα που έχουν μια ιδιότητα προσαρμοσμένου **αριθμού μηνύματος** που είναι μεγαλύτερη από 3:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ομοίως, το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα `LowMessages` με **SqlFilter** που επιλέγει μόνο τα μηνύματα που έχουν μια **αριθμού μηνύματος** ιδιότητα μικρότερη ή ίση με 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Όταν ένα μήνυμα αποστέλλεται τώρα για να `MyTopic`, παραδίδονται πάντα σε μια συνδρομή στο δέκτες το `AllMessages` συνδρομή το θέμα, και να παραδοθεί επιλεκτικής δέκτες μια συνδρομή στο το `HighMessages` και `LowMessages` συνδρομές θέμα (ανάλογα με το περιεχόμενο του μηνύματος).

## <a name="how-to-send-messages-to-a-topic"></a>Πώς μπορείτε να στείλετε μηνύματα σε ένα θέμα

Για να στείλετε ένα μήνυμα σε ένα θέμα Bus υπηρεσίας, η εφαρμογή σας πρέπει να χρησιμοποιήσετε τη μέθοδο **sendTopicMessage** του αντικειμένου **ServiceBusService** .
Τα μηνύματα που αποστέλλονται σε θέματα υπηρεσίας Bus είναι **BrokeredMessage** αντικείμενα.
Τα αντικείμενα **BrokeredMessage** έχουν ένα σύνολο τυπικές ιδιότητες (όπως **ετικέτα** και **το TimeToLive**), ένα λεξικό που χρησιμοποιείται για τη διατήρηση προσαρμοσμένη εφαρμογή συγκεκριμένες ιδιότητες, και ένα σώμα δεδομένων συμβολοσειρά. Μια εφαρμογή να ορίσετε το σώμα του μηνύματος, μεταφέροντας μια τιμή συμβολοσειρά για την **sendTopicMessage** και οποιεσδήποτε τυπικές απαιτούμενες ιδιότητες θα συμπληρώνεται με προεπιλεγμένες τιμές.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να στείλετε πέντε δοκιμή μηνυμάτων σε 'MyTopic'. Σημειώστε ότι η τιμή της ιδιότητας **αριθμού μηνύματος** για κάθε μήνυμα ποικίλλει σε την επανάληψη του βρόχου (αυτό θα καθορίσει ποιες συνδρομές λαμβάνουν την):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Θέματα υπηρεσίας Bus υποστηρίζει ένα μέγιστο μέγεθος μηνύματος 256 KB η [τυπική σειρά](service-bus-premium-messaging.md) και 1 MB στο [επίπεδο Premium](service-bus-premium-messaging.md). Την επικεφαλίδα, η οποία περιλαμβάνει τις ιδιότητες τυπικών και προσαρμοσμένων εφαρμογών, μπορεί να έχει ένα μέγιστο μέγεθος των 64 KB. Υπάρχει όριο στον αριθμό των μηνυμάτων που θα διατηρούνται σε ένα θέμα, αλλά υπάρχει μια καπέλο το συνολικό μέγεθος των μηνυμάτων που διαθέτει ένα θέμα. Σε αυτό το θέμα μέγεθος ορίζεται κατά τη δημιουργία, με ένα ανώτερο όριο των 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Λήψη μηνυμάτων από μια συνδρομή

Λήψη των μηνυμάτων από μια συνδρομή με τη μέθοδο του **receiveSubscriptionMessage** στο αντικείμενο **ServiceBusService** . Από προεπιλογή, τα μηνύματα διαγράφονται από τη συνδρομή όπως διαβάζονται; Ωστόσο, μπορείτε να διαβάσετε (σύνοψη) και να κλειδώσετε το μήνυμα χωρίς να το διαγράψετε από τη συνδρομή, ορίζοντας την προαιρετική παράμετρος **isPeekLock** στην **τιμή true**.

Η προεπιλεγμένη συμπεριφορά της ανάγνωσης και τη διαγραφή του μηνύματος ως μέρος του η λειτουργία λήψης είναι απλούστερο μοντέλο και λειτουργεί καλύτερα για σενάρια στις οποίες μια εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή η υπηρεσία Bus θα έχουν σήμανση του μηνύματος ως που καταναλώνεται, στη συνέχεια, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, το θα έχετε παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Εάν η παράμετρος **isPeekLock** έχει οριστεί στην **τιμή true**, η λήψη γίνεται μια λειτουργία δύο στάδιο, γεγονός που επιτρέπει σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει μια αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλους καταναλωτές πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή.
Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής κλήση μεθόδου **deleteMessage** και παρέχοντας το μήνυμα που θα διαγραφεί ως παράμετρο. Η μέθοδος **deleteMessage** θα επισημάνετε το μήνυμα ως που καταναλώνεται και να την καταργήσετε από τη συνδρομή.

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορούν να ληφθούν τα μηνύματα και να υποβάλλονται σε επεξεργασία με χρήση **receiveSubscriptionMessage**. Το παράδειγμα πρώτα λαμβάνει και διαγράφει ένα μήνυμα από τη συνδρομή 'LowMessages' και, στη συνέχεια, να λάβει ένα μήνυμα από τη συνδρομή 'HighMessages' χρήση **isPeekLock** οριστεί στην τιμή true. Στη συνέχεια, διαγράφει το μήνυμα χρησιμοποιώντας **deleteMessage**:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Πώς να χειριστείτε σφάλματα εφαρμογών και αναγνώσιμα μηνύματα

Υπηρεσία Bus παρέχει λειτουργικότητα που θα σας βοηθήσουν να ανακτήσετε ομαλά από σφάλματα στο εφαρμογής ή δυσκολίες επεξεργασία ενός μηνύματος. Εάν μια εφαρμογή ακουστικό είναι δυνατή η επεξεργασία του μηνύματος για κάποιο λόγο, στη συνέχεια, το να καλέσετε τη μέθοδο **unlockMessage** στο αντικείμενο **ServiceBusService** . Αυτό θα προκαλέσει Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα εντός της συνδρομής και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με την ίδια εφαρμογή καταναλώνει ή από άλλη εφαρμογή καταναλώνει.

Υπάρχει επίσης ένα χρονικό όριο που σχετίζεται με ένα μήνυμα κλειδωμένο εντός της συνδρομής και, εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν από την κλείδωμα λήξει το χρονικό όριο (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus καταργεί αυτόματα το μήνυμα και κάνει διαθέσιμο θα ληφθεί ξανά.

Σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την κλήση της μεθόδου **deleteMessage** , στη συνέχεια, το μήνυμα θα είναι redelivered στην εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται **Τουλάχιστον αφού επεξεργασίας**, αυτό σημαίνει ότι θα υποβληθούν τουλάχιστον μία φορά σε κάθε μήνυμα αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, προγραμματιστές εφαρμογών πρέπει να Προσθήκη επιπλέον λογικής σε εφαρμογή τους χειρισμού διπλότυπων μήνυμα παράδοσης. Συχνά, αυτό είναι δυνατό χρησιμοποιώντας την ιδιότητα **αναγνωριστικού μηνύματος** του μηνύματος, η οποία θα παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης.

## <a name="delete-topics-and-subscriptions"></a>Διαγράψτε τα θέματα και συνδρομών

Θέματα και συνδρομές είναι μόνιμες και πρέπει να διαγραφούν ρητά είτε μέσω του [Azure κλασική πύλη][] ή μέσω προγραμματισμού.
Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε το θέμα με το όνομα `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Διαγραφή ενός θέματος, θα διαγραφούν επίσης τις συνδρομές που έχουν καταχωρηθεί με το θέμα. Συνδρομές μπορεί επίσης να διαγραφεί ανεξάρτητα. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε μια εγγραφή με όνομα `HighMessages` από το `MyTopic` το θέμα:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία για τα θέματα Bus υπηρεσίας, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

-   Ανατρέξτε στο θέμα [ουρές, θέματα, και συνδρομών][].
-   Αναφορά API για [SqlFilter][].
-   Επισκεφθείτε το αποθετήριο [Azure SDK για κόμβο][] στο GitHub.

  [Azure SDK για κόμβου]: https://github.com/Azure/azure-sdk-for-node
  [Azure κλασική πύλη]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Ουρές, θέματα και συνδρομών]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Υπηρεσία node.js Cloud]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Δημιουργία και ανάπτυξη μιας εφαρμογής Node.js σε μια τοποθεσία Web Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Υπηρεσία node.js Cloud με το χώρο αποθήκευσης]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Εφαρμογή node.js Web με χώρου αποθήκευσης]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 

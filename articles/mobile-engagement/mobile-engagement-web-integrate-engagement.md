<properties
    pageTitle="Azure ενοποίησης Mobile δέσμευση Web SDK | Microsoft Azure"
    description="Τις πιο πρόσφατες ενημερώσεις και διαδικασίες για το Azure Mobile δέσμευση Web SDK"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Ενοποίηση του Azure Mobile δέσμευση σε μια εφαρμογή web

> [AZURE.SELECTOR]
- [Παγκόσμια των Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Οι διαδικασίες σε αυτό το άρθρο περιγράφουν ο πιο απλός τρόπος για να ενεργοποιήσετε την ανάλυση και παρακολούθησης συναρτήσεις στο Azure Mobile δέσμευση στην εφαρμογή web σας.

Ακολουθήστε τα βήματα για να ενεργοποιήσετε τις αναφορές αρχείου καταγραφής που είναι απαραίτητα για τον υπολογισμό όλα τα στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και technicals. Για τις στατιστικές εξαρτάται από την εφαρμογή, όπως τα συμβάντα, σφάλματα και εργασίες, πρέπει να ενεργοποιήσετε αναφορές αρχείου καταγραφής με μη αυτόματο τρόπο χρησιμοποιώντας το API Azure Mobile δέσμευση. Για περισσότερες πληροφορίες, μάθετε [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API σε μια εφαρμογή web](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Εισαγωγή

[Λήψη Web Azure κινητή δέσμευση SDK](http://aka.ms/P7b453).
Το κινητό δέσμευση Web SDK αποστέλλεται ως ένα μόνο αρχείο JavaScript, azure-engagement.js, που πρέπει να συμπεριλάβετε σε κάθε σελίδα της εφαρμογής σας τοποθεσία ή web.

> [AZURE.IMPORTANT] Πριν να εκτελέσετε αυτήν τη δέσμη ενεργειών, πρέπει να εκτελέσετε μια δέσμη ενεργειών ή κώδικα τμημάτων κώδικα που συντάσσετε για ρύθμιση παραμέτρων δέσμευση Mobile για την εφαρμογή σας.

## <a name="browser-compatibility"></a>Συμβατότητα προγράμματος περιήγησης

Το SDK Mobile δέσμευση Web χρησιμοποιεί εγγενούς JSON κωδικοποίησης και αποκωδικοποίησης, εκτός από αιτήσεις AJAX μεταξύ τομέων (βασίζεστε στο της προδιαγραφής W3C CORS). Είναι συμβατή με τα ακόλουθα προγράμματα περιήγησης:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Ρύθμιση παραμέτρων κινητής δέσμευση

Συντάξτε μια δέσμη ενεργειών που δημιουργεί καθολικός `azureEngagement` αντικείμενο JavaScript, όπως στο παρακάτω παράδειγμα. Επειδή η τοποθεσία σας ενδέχεται να έχει πολλαπλά στοιχεία σελίδες, αυτό το παράδειγμα προϋποθέτει ότι αυτή η δέσμη ενεργειών περιλαμβάνεται σε κάθε σελίδα. Σε αυτό το παράδειγμα, με το όνομα του αντικειμένου JavaScript `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Το `connectionString` τιμή για την εφαρμογή σας εμφανίζεται στην πύλη του Azure.

> [AZURE.NOTE] `appVersionName`και `appVersionCode` είναι προαιρετικό. Ωστόσο, συνιστάται να που ρυθμίζετε τις παραμέτρους τους ώστε να ανάλυσης μπορεί να επεξεργαστεί πληροφορίες έκδοσης.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Συμπερίληψη δέσμευση Mobile δέσμες ενεργειών σε σελίδες σας
Προσθήκη δέσμευσης Mobile δέσμες ενεργειών στις σελίδες σας με έναν από τους εξής τρόπους:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Ή αυτό:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Ψευδώνυμο

Μετά τη φόρτωση της δέσμης ενεργειών Mobile δέσμευση Web SDK, που δημιουργεί το ψευδώνυμο **Δέσμευση** για να αποκτήσετε πρόσβαση τα API SDK. Δεν μπορείτε να χρησιμοποιήσετε αυτό το ψευδώνυμο για να ορίσετε τη ρύθμιση παραμέτρων SDK. Αυτό το ψευδώνυμο χρησιμοποιείται ως αναφορά σε αυτήν την τεκμηρίωση.

Λάβετε υπόψη ότι εάν το προεπιλεγμένο ψευδώνυμο έρχεται σε διένεξη με μια άλλη μεταβλητή καθολικού από τη σελίδα σας, μπορείτε να επανακαθορίσετε το στη ρύθμιση παραμέτρων ως εξής πριν να φορτώσετε το SDK Mobile δέσμευση Web:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Βασική αναφορά

Βασική αναφορά στο κινητό δέσμευση καλύπτει την περίοδο λειτουργίας επιπέδου στατιστικά στοιχεία, όπως στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, δραστηριότητες και παρουσιάζει σφάλμα.

### <a name="session-tracking"></a>Παρακολούθηση της περιόδου λειτουργίας

Μια περίοδο λειτουργίας δέσμευση Mobile χωρίζεται σε μια ακολουθία δραστηριοτήτων, κάθε αναγνωρίζονται από το όνομα.

Σε κλασική Web, συνιστάται να δηλώσετε μια διαφορετική δραστηριότητα σε κάθε σελίδα της τοποθεσίας σας. Για μια τοποθεσία Web ή μιας εφαρμογής web στην οποία δεν αλλάζει ποτέ στην τρέχουσα σελίδα, ενδέχεται να θέλετε να παρακολουθείτε τις δραστηριότητες σε μικρότερες κλίμακα, όπως σε μια σελίδα.

Σε κάθε περίπτωση, για να ξεκινήσετε ή να αλλάξετε την τρέχουσα δραστηριότητά του χρήστη, καλέστε την `engagement.agent.startActivity` συνάρτηση. Για παράδειγμα:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Ο διακομιστής δέσμευση Mobile τερματίζεται αυτόματα μια ανοιχτή περίοδο λειτουργίας μέσα σε τρία λεπτά μετά από τη σελίδα της εφαρμογής είναι κλειστό.

Εναλλακτικά, μπορείτε να τερματίσετε μια περίοδο λειτουργίας με μη αυτόματο τρόπο, καλώντας `engagement.agent.endActivity`. Αυτό ρυθμίζει αυτή η ενέργεια χρήστη να 'Αδράνειας'.  Η περίοδος λειτουργίας θα τερματιστεί 10 δευτερόλεπτα αργότερα, εκτός εάν μια νέα κλήση σε `engagement.agent.startActivity` ισχύει την περίοδο λειτουργίας.

Μπορείτε να ρυθμίσετε την καθυστέρηση 10 δευτερολέπτων στο αντικείμενο καθολικού δέσμευση, ως εξής:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Δεν μπορείτε να χρησιμοποιήσετε `engagement.agent.endActivity` στο το `onunload` επιστροφής κλήσης επειδή δεν μπορείτε να κάνετε κλήσεις AJAX σε αυτό το στάδιο.

## <a name="advanced-reporting"></a>Σύνθετες αναφοράς

Προαιρετικά, εάν θέλετε να αναφέρετε συγκεκριμένη εφαρμογή συμβάντα, σφάλματα και εργασίες, πρέπει να χρησιμοποιήσετε το API δέσμευση Mobile. Έχετε πρόσβαση το API δέσμευση Mobile έως το `engagement.agent` αντικειμένου.

Μπορείτε να αποκτήσετε πρόσβαση σε όλες τις δυνατότητες για προχωρημένους στο κινητό δέσμευση στο API δέσμευση Mobile. Το API περιγράφεται στο άρθρο [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API σε μια εφαρμογή web](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>Προσαρμόστε τις διευθύνσεις URL που χρησιμοποιούνται για κλήσεις AJAX

Μπορείτε να προσαρμόσετε τις διευθύνσεις URL που χρησιμοποιεί το SDK Mobile δέσμευση Web. Για παράδειγμα, για να καθορίσετε πάλι τη διεύθυνση URL του αρχείου καταγραφής (το τελικό σημείο SDK για τη σύνδεση), μπορείτε να παρακάμψετε τη ρύθμιση παραμέτρων ως εξής:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Εάν σας διεύθυνση URL συναρτήσεις επιστρέφουν μια συμβολοσειρά που αρχίζει με `/`, `//`, `http://`, ή `https://`, ο προεπιλεγμένος συνδυασμός δεν χρησιμοποιείται. Από προεπιλογή, το `https://` συνδυασμού χρησιμοποιείται για αυτές τις διευθύνσεις URL. Εάν θέλετε να προσαρμόσετε το προεπιλεγμένο συνδυασμό, αντικαταστήσει τη ρύθμιση παραμέτρων, ως εξής:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };

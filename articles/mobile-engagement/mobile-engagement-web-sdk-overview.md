<properties
    pageTitle="Επισκόπηση SDK Web Azure κινητή δέσμευση | Microsoft Azure"
    description="Τις πιο πρόσφατες ενημερώσεις και διαδικασίες για το SDK Web για δέσμευση Mobile Azure"
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
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure κινητή δέσμευση Web SDK

Ξεκινήστε εδώ για όλες τις λεπτομέρειες σχετικά με το πώς μπορείτε να ενοποιήσετε Azure Mobile δέσμευση σε μια εφαρμογή web. Εάν θέλετε να του δώσετε δοκιμή πριν γρήγορα αποτελέσματα με το δικό σας web app, ανατρέξτε στο θέμα μας [Εκμάθηση διάρκειας 15 λεπτών](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Διαδικασίες ολοκλήρωσης
1. Μάθετε [πώς μπορείτε να ενοποιήσετε δέσμευση Mobile στην εφαρμογή web](mobile-engagement-web-integrate-engagement.md).

2. Για εφαρμογή πρόγραμμα ετικέτας, μάθετε [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή web](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Σημειώσεις έκδοσης

### <a name="202-10182016"></a>2.0.2 (18/10/2016)

-   Σταθερή σφάλμα στην ιδιωτική περιήγηση (Safari).
-   Σταθερή σφάλμα σε προγράμματα περιήγησης με απενεργοποιήσει τα cookies.

Για όλες τις εκδόσεις, ανατρέξτε στις [Σημειώσεις έκδοσης ολοκλήρωσης](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Διαδικασίες αναβάθμισης

### <a name="upgrade-from-121-to-200"></a>Αναβάθμιση από 1.2.1 σε 2.0.0

Οι παρακάτω ενότητες περιγράφουν τον τρόπο για να μετεγκαταστήσετε μια ενοποίηση Mobile δέσμευση Web SDK από την υπηρεσία Capptain, που παρέχεται από Capptain συσχετισμών Ασφαλείας, σε μια εφαρμογή της δέσμευσης Mobile Azure. Εάν κάνετε μετεγκατάσταση από μια έκδοση παλαιότερη από το 1.2.1, συμβουλευτείτε την τοποθεσία Web Capptain για τη μετεγκατάσταση στο 1.2.1 πρώτα και, στη συνέχεια, εφαρμόστε τις παρακάτω διαδικασίες.

Αυτή η έκδοση του SDK Mobile δέσμευση Web δεν υποστηρίζει έξυπνη Τηλεοπτική Samsung, Τηλεοπτική Opera, webOS ή τη δυνατότητα Reach.

>[AZURE.IMPORTANT] Capptain και δέσμευση Mobile Azure δεν είναι η ίδια υπηρεσία και τις ακόλουθες διαδικασίες επισήμανση μόνο με τη μετεγκατάσταση στην εφαρμογή υπολογιστή-πελάτη. Μετεγκατάσταση στο SDK Mobile δέσμευση Web στην εφαρμογή θα μετεγκατασταθεί τα δεδομένα σας από ένα διακομιστή Capptain σε ένα διακομιστή δέσμευση Mobile.

#### <a name="javascript-files"></a>Αρχεία JavaScript

Αντικαταστήστε το αρχείο capptain-sdk.js με το azure-engagement.js αρχείο και, στη συνέχεια, ενημερώστε τις εισαγωγές δέσμης ενεργειών αντίστοιχα.

#### <a name="remove-capptain-reach"></a>Κατάργηση Capptain Reach

Αυτή η έκδοση του SDK Mobile δέσμευση Web δεν υποστηρίζει τη δυνατότητα Reach. Εάν έχετε συνδέσει Capptain Reach στην εφαρμογή σας, πρέπει να την καταργήσετε.

Κατάργηση την εισαγωγή επίτευξη CSS από τη σελίδα και διαγράψτε το αρχείο .css σχετικές (capptain-reach.css, από προεπιλογή).

Διαγραφή στους παρακάτω πόρους Reach: Κλείσιμο εικόνας (capptain-close.png, από προεπιλογή) και το εικονίδιο της εμπορικής επωνυμίας (capptain--εικονίδιο ειδοποίησης, από προεπιλογή).

Κατάργηση φτάσει στο περιβάλλον εργασίας Χρήστη του για τις ειδοποιήσεις της εφαρμογής. Η προεπιλεγμένη διάταξη μοιάζει κάπως έτσι:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Καταργήστε την επίτευξη περιβάλλοντος εργασίας Χρήστη για το κείμενο και web ανακοινώσεων και ψηφοφορίες. Η προεπιλεγμένη διάταξη μοιάζει κάπως έτσι:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Κατάργηση του `reach` αντικείμενο από τις ρυθμίσεις σας, εάν υπάρχει. Μοιάζει κάπως έτσι:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Καταργήστε τυχόν άλλες Προσαρμογή Reach, όπως οι κατηγορίες.

#### <a name="remove-deprecated-apis"></a>Κατάργηση APIs που έχουν καταργηθεί

Ορισμένες APIs από Capptain έχουν καταργηθεί στο SDK Mobile δέσμευση Web.

Καταργήστε τυχόν κλήσεις για τα παρακάτω API: `agent.connect`, `agent.disconnect`, `agent.pause`, και `agent.sendMessageToDevice`.

Κατάργηση οποιοδήποτε από τα εξής επιστροφές κλήσης από της ρύθμισης παραμέτρων Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, και `onPushMessageReceived`.

#### <a name="configuration"></a>Ρύθμιση παραμέτρων

Δέσμευση Mobile χρησιμοποιεί μια συμβολοσειρά σύνδεσης για να ρυθμίσετε τις παραμέτρους αναγνωριστικά SDK, για παράδειγμα, το αναγνωριστικό εφαρμογής.

Αντικαταστήστε το Αναγνωριστικό εφαρμογής με τη συμβολοσειρά σύνδεσης. Σημειώστε ότι οι αλλαγές στο καθολικό αντικείμενο για τη ρύθμιση παραμέτρων SDK από `capptain` να `azureEngagement`.

Πριν από την μετεγκατάσταση:

    window.capptain = {
      appId: ...,
      [...]
    };

Μετά τη μετεγκατάσταση:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Εμφανίζεται η συμβολοσειρά σύνδεσης για την εφαρμογή σας στην πύλη του Azure.

#### <a name="javascript-apis"></a>JavaScript APIs

Το αντικείμενο JavaScript καθολικού `window.capptain` έχει μετονομαστεί `window.azureEngagement`, αλλά μπορείτε να χρησιμοποιήσετε το `window.engagement` ψευδώνυμο για κλήσεις API. Δεν μπορείτε να χρησιμοποιήσετε αυτό το ψευδώνυμο για να ορίσετε τη ρύθμιση παραμέτρων SDK.

Για παράδειγμα, `capptain.deviceId` γίνεται `engagement.deviceId`, `capptain.agent.startActivity` γίνεται `engagement.agent.startActivity`, και ούτω καθεξής.

Εάν έχετε συνδέσει μια παλαιότερη έκδοση του Azure Mobile δέσμευση Web SDK ήδη στην εφαρμογή σας, διαβάστε σχετικά με την [αναβάθμιση διαδικασίες](mobile-engagement-web-upgrade-procedure.md).

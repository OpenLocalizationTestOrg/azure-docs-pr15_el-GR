<properties
    pageTitle="Προσθέστε τις ειδοποιήσεις Push Apache Cordova εφαρμογή με το Azure εφαρμογές για κινητές συσκευές | Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς να χρησιμοποιείτε εφαρμογές του Mobile Azure για την αποστολή ειδοποιήσεων push σε εφαρμογή Apache Cordova."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Προσθέστε τις ειδοποιήσεις push σε εφαρμογή Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Επισκόπηση

Σε αυτό το πρόγραμμα εκμάθησης, προσθέτετε τις ειδοποιήσεις push στο έργο [Cordova Apache Γρήγορη εκκίνηση] , έτσι ώστε να αποστέλλεται μια ειδοποίηση push στη συσκευή κάθε φορά που έχει εισαχθεί μια εγγραφή.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, θα χρειαστεί το πακέτο επέκτασης ειδοποιήσεων push. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης καλύπτει μια εφαρμογή Apache Cordova που αναπτύχθηκε με οπτική 2015 Studio που εκτελείται στον του Android προσομοίωσης Google, μια συσκευή Android, μια συσκευή Windows και συσκευή iOS.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει:

* Έναν Υπολογιστή με το [Visual Studio Κοινότητας 2015] ή νεότερες εκδόσεις.
* Τα [Εργαλεία του visual Studio για Apache Cordova].
* Ένας [λογαριασμός Azure active](https://azure.microsoft.com/pricing/free-trial/).
* Ένα ολοκληρωμένο έργο [Cordova Apache Γρήγορη εκκίνηση] .
* (Android) Ένας [λογαριασμός Google] με μια διεύθυνση ηλεκτρονικού ταχυδρομείου επαληθευτεί.
* (iOS) Μια ιδιότητα μέλους Apple προγραμματιστής προγράμματος και συσκευή iOS (iOS Simulator δεν υποστηρίζει push).
* (Windows) Ένα λογαριασμό Windows Store προγραμματιστής και συσκευή Windows 10.

##<a name="configure-hub"></a>Ρύθμιση παραμέτρων διανομέα ειδοποίησης

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Παρακολουθήστε ένα βίντεο που δείχνει τα βήματα σε αυτήν την ενότητα](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Ενημέρωση του project server για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Τροποποιήστε την εφαρμογή Cordova σας να λαμβάνει τις ειδοποιήσεις push

Πρέπει να βεβαιωθείτε ότι το έργο σας Apache Cordova εφαρμογή είναι έτοιμος να χειρίζεται τις ειδοποιήσεις push κατά την εγκατάσταση της προσθήκης push Cordova συν οποιεσδήποτε υπηρεσίες push συγκεκριμένης πλατφόρμας.

#### <a name="update-the-cordova-version-in-your-project"></a>Ενημερώστε την έκδοση Cordova στο έργο σας.

Συνιστάται να ενημερώσετε το πρόγραμμα-πελάτη project με Cordova 6.1.1 εάν το έργο σας έχει ρυθμιστεί ώστε να χρησιμοποιεί μια παλαιότερη έκδοση. Για να ενημερώσετε το έργο, κάντε δεξί κλικ config.xml για να ανοίξετε το εργαλείο σχεδίασης ρύθμισης παραμέτρων. Επιλέξτε την καρτέλα πλατφόρμες και επιλέξτε 6.1.1 στο πλαίσιο κειμένου **Cordova CLI** .

Επιλέξτε **Δημιουργία**, στη συνέχεια, **Δημιουργήστε λύση** για να ενημερώσετε το έργο.

#### <a name="install-the-push-plugin"></a>Εγκατάσταση της προσθήκης push

Εφαρμογές Apache Cordova δεν χειρίζονται εγγενώς δυνατότητες δικτύου ή συσκευής.  Αυτές οι δυνατότητες παρέχονται από προσθήκες που δημοσιεύονται σε [npm](https://www.npmjs.com/) ή σε GitHub.  Το `phonegap-plugin-push` προσθήκης χρησιμοποιείται για να χειριστείτε τις ειδοποιήσεις push δικτύου.

Μπορείτε να εγκαταστήσετε την προσθήκη push με έναν από τους εξής τρόπους:

**Από τη γραμμή εντολών:**

Εκτελέστε την ακόλουθη εντολή:

    cordova plugin add phonegap-plugin-push

**Από μέσα σε Visual Studio:**

1.  Στην Εξερεύνηση λύσεων, ανοίξτε το `config.xml` αρχείο, κάντε κλικ στην επιλογή **προσθήκες** > **Προσαρμογή**, επιλέξτε **Git** ως την προέλευση της εγκατάστασης, στη συνέχεια, πληκτρολογήστε `https://github.com/phonegap/phonegap-plugin-push` ως προέλευση.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Κάντε κλικ στο βέλος δίπλα στην επιλογή την προέλευση της εγκατάστασης.

3. Στο **SENDER_ID**, εάν έχετε ήδη ένα Αναγνωριστικό αριθμητική έργου για το έργο κονσόλα Google προγραμματιστής, μπορείτε να την προσθέσετε εδώ. Διαφορετικά, εισαγάγετε μια τιμή κράτησης θέσης, όπως η 777777, και εάν στοχεύετε σε Android, μπορείτε να ενημερώσετε αυτήν την τιμή στο config.xml αργότερα.

4. Κάντε κλικ στην επιλογή **Προσθήκη**.

Η προσθήκη push τώρα είναι εγκατεστημένη.

####<a name="install-the-device-plugin"></a>Εγκατάσταση της προσθήκης συσκευής

Ακολουθήστε την ίδια διαδικασία που χρησιμοποιήσατε για να εγκαταστήσετε την προσθήκη push, αλλά θα βρείτε την προσθήκη συσκευή στη λίστα προσθηκών πυρήνα (κάντε κλικ στην επιλογή **προσθήκες** > **πυρήνα** για να το βρείτε). Πρέπει να αποκτήσετε το όνομα της πλατφόρμας αυτή η προσθήκη (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Καταχώρηση τη συσκευή σας για push σε εκκίνησης

Αρχικά, θα σας θα περιλαμβάνει ορισμένες ελάχιστο κώδικα για Android. Αργότερα, θα κάνουμε ορισμένες μικρές τροποποιήσεις για να εκτελέσετε σε iOS ή Windows 10.

1. Προσθέστε μια κλήση για να **registerForPushNotifications** κατά τη διάρκεια της επιστροφής κλήσης για τη διαδικασία σύνδεσης ή στο κάτω μέρος της μεθόδου **onDeviceReady** :

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Αυτό το παράδειγμα εμφανίζει κλήση **registerForPushNotifications** όταν ολοκληρωθεί με επιτυχία ο έλεγχος ταυτότητας, που συνιστώνται όταν χρησιμοποιείτε τις ειδοποιήσεις push και τον έλεγχο ταυτότητας στην εφαρμογή.

2. Προσθέστε τη νέα μέθοδο **registerForPushNotifications** ως εξής:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) Στο παραπάνω κώδικα, αντικαταστήστε `Your_Project_ID` με το αριθμητικό project Αναγνωριστικό για την εφαρμογή σας από την [Κονσόλα Google προγραμματιστής].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Προαιρετικό) Ρύθμιση παραμέτρων και εκτελέστε την εφαρμογή σε Android

Ολοκληρώστε αυτήν την ενότητα για να ενεργοποιήσετε τις ειδοποιήσεις push για Android.

####<a name="enable-gcm"></a>Ενεργοποίηση Firebase στο Cloud ανταλλαγής μηνυμάτων

Εφόσον μας στοχεύετε της πλατφόρμας Google Android αρχικά, πρέπει να ενεργοποιήσετε το Firebase Cloud μηνυμάτων. Ομοίως, εάν στόχευσης συσκευές Microsoft Windows, μπορείτε να ενεργοποιήσετε την υποστήριξη WNS.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>Ρύθμιση παραμέτρων υπόβαθρο εφαρμογή Mobile για να στείλετε προσκλήσεις push χρησιμοποιώντας FCM

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Ρύθμιση παραμέτρων της εφαρμογής σας Cordova για Android

Στην εφαρμογή Cordova, ανοίξετε config.xml και να αντικαταστήσετε `Your_Project_ID` με το αριθμητικό project Αναγνωριστικό για την εφαρμογή σας από την [Κονσόλα Google προγραμματιστής].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Ανοίξτε το index.js και ενημερώστε τον κώδικα για να χρησιμοποιήσετε το αναγνωριστικό αριθμητική έργου.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Ρύθμιση παραμέτρων συσκευή Android για τον εντοπισμό σφαλμάτων USB

Για να αναπτύξετε την εφαρμογή σας σε συσκευή Android, πρέπει να ενεργοποιήσετε τον εντοπισμό σφαλμάτων USB.  Εκτελέστε τα ακόλουθα βήματα στο τηλέφωνο Android:

1. Μεταβείτε στις **Ρυθμίσεις** > **σχετικά με το τηλέφωνο**, στη συνέχεια, πατήστε **αριθμός έκδοσης** μέχρι την κατάσταση λειτουργίας προγραμματιστή είναι ενεργοποιημένο (περίπου 7 φορές).

2. Πίσω στο **Ρυθμίσεις** > **Επιλογές προγραμματιστή** ενεργοποιήσετε **τον εντοπισμό σφαλμάτων USB**, στη συνέχεια, συνδεθείτε τηλέφωνο Android σας ανάπτυξης του Υπολογιστή σας με το καλώδιο USB.

Κάναμε δοκιμές αυτό χρησιμοποιώντας μια συσκευή X Google Nexus 5 εκτελείται Android 6.0 (Marshmallow).  Ωστόσο, οι τεχνικές είναι κοινά σε οποιαδήποτε Μοντέρνο Android έκδοση.

#### <a name="install-google-play-services"></a>Εγκαταστήστε τις υπηρεσίες Google Play

Η προσθήκη push βασίζεται σε Android το Google Play υπηρεσίες για τις ειδοποιήσεις προώθησης.  

1.  Στο **Visual Studio**, κάντε κλικ στην επιλογή **Εργαλεία** > **Android** > **Android SDK Manager**, αναπτύξτε το φάκελο **πρόσθετα** και επιλέξτε το πλαίσιο για να βεβαιωθείτε ότι κάθε ένα από τα εξής SDK είναι εγκατεστημένη.
    * Android 2.3 ή νεότερη έκδοση
    * Αναθεώρηση αποθετήριο Google 27 ή νεότερη έκδοση
    * Υπηρεσίες Google Play 9.0.2 ή νεότερη έκδοση

2.  Κάντε κλικ στην **Εγκατάσταση πακέτων** και περιμένετε να ολοκληρωθεί η εγκατάσταση.

Η τρέχουσα απαιτείται βιβλιοθήκες παρατίθενται στην [τεκμηρίωση phonegap-Προσθήκη-push εγκατάστασης].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή σε Android

Μπορείτε να τις ειδοποιήσεις push δοκιμής τώρα, εκτελώντας την εφαρμογή και εισαγωγή στοιχείων του πίνακα TodoItem. Μπορείτε να το κάνετε από την ίδια συσκευή ή από μια δεύτερη συσκευή, με την προϋπόθεση ότι χρησιμοποιείτε το ίδιο παρασκηνίου. Ελέγξτε την εφαρμογή σας Cordova την πλατφόρμα Android με έναν από τους εξής τρόπους:

- **Σε φυσική συσκευή:**  
Συνδέστε τη συσκευή Android στον υπολογιστή σας στην ανάπτυξη με το καλώδιο USB.  Αντί για **Android προσομοίωσης Google**, επιλέξτε **τη συσκευή**. Visual Studio θα ανάπτυξη της εφαρμογής στη συσκευή και να εκτελέσετε.  Στη συνέχεια, μπορείτε να αλληλεπιδράτε με την εφαρμογή στη συσκευή.  
Βελτιώστε την εμπειρία σας με ανάπτυξη.  Κοινής χρήσης εφαρμογές, όπως το [Mobizen] οθόνης μπορούν να σας βοηθήσουν στην ανάπτυξη μιας εφαρμογής Android από την προβολή σας Android οθόνη με ένα πρόγραμμα περιήγησης web στον Υπολογιστή σας.

- **Σε ένα Android προσομοίωσης:**  
Υπάρχουν επιπλέον βήματα ρύθμισης παραμέτρων απαιτούνται όταν εκτελείται σε ένα προσομοίωσης.

    Βεβαιωθείτε ότι για την ανάπτυξη στον ή τον εντοπισμό σφαλμάτων σε μια εικονική συσκευή που έχει APIs Google Ορισμός ως προορισμού, όπως φαίνεται παρακάτω στη Διαχείριση Android εικονική συσκευή (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Εάν θέλετε να χρησιμοποιήσετε μια ταχύτερη x86 προσομοίωσης, μπορείτε να [εγκαταστήσετε το πρόγραμμα οδήγησης HAXM](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) και ρυθμίστε τις παραμέτρους του προσομοίωσης το χρησιμοποιήσετε.

    Προσθέστε ένα λογαριασμό Google στη συσκευή Android, κάνοντας κλικ στην επιλογή **εφαρμογές** > **Ρυθμίσεις** > **Προσθήκη λογαριασμού**, στη συνέχεια, ακολουθήστε τις οδηγίες για να προσθέσετε μια υπάρχουσα Google λογαριασμού στη συσκευή (συνιστάται να χρησιμοποιείτε έναν υπάρχοντα λογαριασμό, αντί να δημιουργήσετε ένα νέο).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Εκτελέστε την εφαρμογή todolist ως πριν από και εισαγάγετε ένα νέο στοιχείο todo. Αυτήν τη στιγμή, εμφανίζεται ένα εικονίδιο ειδοποίησης στην περιοχή ειδοποιήσεων. Μπορείτε να ανοίξετε το σχέδιο ειδοποίησης για να προβάλετε το πλήρες κείμενο της ειδοποίησης.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Προαιρετικό) Ρύθμιση παραμέτρων και την εκτέλεση στο iOS

Αυτή η ενότητα είναι για την εκτέλεση του έργου Cordova σε συσκευές iOS. Μπορείτε να παραλείψετε αυτή την ενότητα, εάν δεν εργάζεστε με συσκευές iOS.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Εγκατάσταση και εκτέλεση τον παράγοντα remotebuild iOS σε μια υπηρεσία Mac ή στο cloud

Πριν μπορέσετε να εκτελέσετε μια εφαρμογή Cordova στη χρήση του Visual Studio iOS, μεταβείτε με τα βήματα του [iOS Οδηγός εγκατάστασης](http://taco.visualstudio.com/en-us/docs/ios-guide/) για να εγκαταστήσετε και να εκτελέσετε τον παράγοντα remotebuild.

Βεβαιωθείτε ότι μπορείτε να δημιουργήσετε την εφαρμογή για iOS. Τα βήματα στον Οδηγό ρύθμισης απαιτούνται για να δημιουργήσετε για iOS από το Visual Studio. Εάν δεν έχετε Mac, μπορείτε να δημιουργήσετε για iOS χρησιμοποιώντας τον παράγοντα remotebuild σε μια υπηρεσία, όπως MacInCloud. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εκτελέστε την εφαρμογή iOS στο cloud](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] Για να χρησιμοποιήσετε την προσθήκη push σε iOS απαιτείται XCode 7 ή μεγαλύτερη.

####<a name="find-the-id-to-use-as-your-app-id"></a>Βρείτε το Αναγνωριστικό για να χρησιμοποιήσετε ως το Αναγνωριστικό εφαρμογής

Πριν από την καταχώρηση της εφαρμογής για τις ειδοποιήσεις push, Άνοιγμα config.xml στην εφαρμογή Cordova, βρείτε το `id` τιμή στο στοιχείο widget χαρακτηριστικού και αντιγράψτε την για μελλοντική χρήση. Στο παρακάτω XML, είναι το Αναγνωριστικό `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Χρησιμοποιήστε αυτό το αναγνωριστικό αργότερα, όταν δημιουργείτε ένα Αναγνωριστικό εφαρμογής στην πύλη για προγραμματιστές της Apple. (Εάν δημιουργείτε διαφορετικό Αναγνωριστικό εφαρμογής στην πύλη προγραμματιστής και θέλετε να χρησιμοποιήσετε, θα πρέπει να ακολουθήσετε μερικά επιπλέον βήματα παρακάτω σε αυτό το πρόγραμμα εκμάθησης για να αλλάξετε αυτό το Αναγνωριστικό σε config.xml. Το Αναγνωριστικό του στοιχείου widget πρέπει να συμφωνεί με το Αναγνωριστικό εφαρμογής στην πύλη για προγραμματιστές.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Καταχώρηση της εφαρμογής για τις ειδοποιήσεις push στην πύλη για προγραμματιστές της Apple

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Παρακολουθήστε ένα βίντεο που δείχνει παρόμοια βήματα](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Ρύθμιση παραμέτρων του Azure για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Βεβαιωθείτε ότι το Αναγνωριστικό εφαρμογής συμφωνεί με την εφαρμογή Cordova

Εάν το Αναγνωριστικό εφαρμογής που δημιουργήσατε στο λογαριασμό σας για προγραμματιστές Apple ήδη συμφωνεί με το Αναγνωριστικό του στοιχείου widget στο config.xml, μπορείτε να παραλείψετε αυτό το βήμα. Ωστόσο, εάν δεν συμφωνούν με τα αναγνωριστικά, ακολουθήστε τα παρακάτω βήματα:

1. Διαγράψτε το φάκελο πλατφόρμες από το έργο σας.

2. Διαγράψτε το φάκελο προσθηκών από το έργο σας.

3. Διαγράψτε το φάκελο node_modules από το έργο σας.

4. Ενημερώστε το χαρακτηριστικό αναγνωριστικό του στοιχείου widget στο αρχείο config.xml για να χρησιμοποιήσετε το Αναγνωριστικό εφαρμογής που δημιουργήσατε στο λογαριασμό σας για προγραμματιστές Apple.

5. Δημιουργήστε ξανά το έργο σας.

#####<a name="test-push-notifications-in-your-ios-app"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή iOS

1. Στο Visual Studio, βεβαιωθείτε ότι είναι επιλεγμένο αυτό **iOS** ως ο προορισμός ανάπτυξης και, στη συνέχεια, επιλέξτε τη **συσκευή** για να εκτελέσετε στη συσκευή σας iOS συνδεδεμένοι.

    Μπορείτε να εκτελέσετε σε συσκευή iOS συνδεδεμένο με τον Υπολογιστή σας χρησιμοποιώντας iTunes. Το iOS Simulator δεν υποστηρίζει τις ειδοποιήσεις προώθησης.

2. Πατήστε το κουμπί **εκτέλεσης** ή **F5** στο Visual Studio για τη δημιουργία του έργου και να ξεκινήσετε την εφαρμογή σε συσκευή iOS και, στη συνέχεια, κάντε κλικ στο **κουμπί OK** για να αποδεχτείτε τις ειδοποιήσεις push.

    >[AZURE.NOTE] Πρέπει να αποδεχτείτε τις ειδοποιήσεις push ρητά από την εφαρμογή. Αυτή η αίτηση προκύπτει μόνο την πρώτη φορά που θα εκτελείται η εφαρμογή.

3. Στην εφαρμογή, πληκτρολογήστε μια εργασία και, στη συνέχεια, κάντε κλικ στο κουμπί συν (+) εικονίδιο.

4. Βεβαιωθείτε ότι μια ειδοποίηση λήψη, στη συνέχεια, κάντε κλικ στο κουμπί OK για να κλείσετε την ειδοποίηση.

##<a name="optional-configure-and-run-on-windows"></a>(Προαιρετικό) Ρύθμιση παραμέτρων και εκτελούνται στα Windows

Αυτή η ενότητα είναι για την εκτέλεση του έργου εφαρμογή Apache Cordova σε συσκευές με Windows 10 (η προσθήκη push PhoneGap υποστηρίζεται στα Windows 10). Εάν δεν εργάζεστε με συσκευές με Windows, μπορείτε να παραλείψετε αυτή την ενότητα.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Καταχώρηση της εφαρμογής Windows για τις ειδοποιήσεις push WNS

Για να χρησιμοποιήσετε τις επιλογές αποθήκευσης στο Visual Studio, επιλέξτε ο στόχος των Windows από τη λίστα λύση πλατφόρμες, όπως το **Windows x64** ή **x86 των Windows** (αποφύγετε **Windows AnyCPU** για τις ειδοποιήσεις push).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Παρακολουθήστε ένα βίντεο που δείχνει παρόμοια βήματα](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Ρύθμιση παραμέτρων ενότητα ειδοποίησης για WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Ρύθμιση παραμέτρων την εφαρμογή σας Cordova για την υποστήριξη των Windows τις ειδοποιήσεις push

Ανοίξτε το εργαλείο σχεδίασης ρύθμισης παραμέτρων (δεξιό κλικ στην επιλογή **Προβολή σχεδίασης**και config.xml), επιλέξτε την καρτέλα **Windows** και επιλέξτε **Windows 10** στην περιοχή **Έκδοση Windows προορισμού**.

>[AZURE.NOTE] Εάν χρησιμοποιείτε μια έκδοση Cordova πριν από την Cordova 5.1.1 (συνιστάται 6.1.1), πρέπει επίσης να ορίσετε τη σημαία με δυνατότητα αναδυόμενη στην τιμή true σε config.xml.

Για την υποστήριξη push ειδοποιήσεις στο προεπιλεγμένο σας (εντοπισμός σφαλμάτων) δημιουργεί build.json ανοιχτό αρχείο. Αντιγράψτε τη ρύθμιση παραμέτρων "τελική έκδοση" ρύθμιση των παραμέτρων σας εντοπισμού σφαλμάτων.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Μετά την ενημέρωση, τον παραπάνω κώδικα θα πρέπει να είναι κάπως έτσι.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Δημιουργία της εφαρμογής και βεβαιωθείτε ότι έχετε χωρίς σφάλματα. Εφαρμογή προγράμματος-πελάτη που τώρα θα πρέπει να εγγραφείτε για τις ειδοποιήσεις από την εφαρμογή Mobile παρασκηνίου. Επαναλάβετε αυτήν την ενότητα για κάθε έργο Windows στη λύση σας.

####<a name="test-push-notifications-in-your-windows-app"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή Windows

Στο Visual Studio, βεβαιωθείτε ότι είναι επιλεγμένο το μια πλατφόρμα Windows ως ο προορισμός ανάπτυξης, όπως το **Windows x64** ή **x86 των Windows**. Για να εκτελέσετε την εφαρμογή υπολογιστή με Windows 10 φιλοξενίας Visual Studio, επιλέξτε **Τοπικό υπολογιστή**.

Πατήστε το κουμπί "Εκτέλεση" για τη δημιουργία του έργου και να ξεκινήσετε την εφαρμογή.

Στην εφαρμογή, πληκτρολογήστε ένα όνομα για μια νέα todoitem και, στη συνέχεια, κάντε κλικ στο κουμπί συν (+) εικονίδιο για να την προσθέσετε.

Βεβαιωθείτε ότι είναι λάβει μια ειδοποίηση όταν προστίθεται το στοιχείο.

##<a name="next-steps"></a>Επόμενα βήματα

* Διαβάστε σχετικά με [Διανομείς ειδοποίηση] για να μάθετε περισσότερα σχετικά με τις ειδοποιήσεις προώθησης.
* Εάν δεν το έχετε κάνει ήδη, συνεχίστε με το πρόγραμμα εκμάθησης, [Προσθέτοντας τον έλεγχο ταυτότητας] για την εφαρμογή σας Apache Cordova.

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το SDK.

* [Apache Cordova SDK]
* [Διακομιστής ASP.NET SDK]
* [Node.js διακομιστή SDK]

<!-- URLs -->
[Προσθήκη ελέγχου ταυτότητας]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova γρήγορης εκκίνησης]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Λογαριασμός Google]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Κονσόλα Google για προγραμματιστές]: https://console.developers.google.com/home/dashboard
[τεκμηρίωση εγκατάστασης phonegap-Προσθήκη-push]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Κοινότητα του Visual Studio 2015]: http://www.visualstudio.com/
[Εργαλεία του Visual Studio για Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Ειδοποίηση διανομείς]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Διακομιστής ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js διακομιστή SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

<properties
    pageTitle="Αποστολή ειδοποιήσεων push με διανομείς ειδοποίηση Azure και Node.js"
    description="Μάθετε πώς να χρησιμοποιείτε διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push από μια εφαρμογή Node.js."
    keywords="ειδοποιήσεων Push, push notifications,node.js push, ios push"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Αποστολή ειδοποιήσεων push με διανομείς ειδοποίηση Azure και Node.js
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Επισκόπηση

> [AZURE.IMPORTANT] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Αυτός ο οδηγός θα σας δείξουν πώς μπορείτε να στείλετε τις ειδοποιήσεις προώθησης με τη Βοήθεια του Azure ειδοποίηση διανομείς απευθείας από μια εφαρμογή Node.js. 

Τα σενάρια καλύπτεται περιλαμβάνουν αποστολή ειδοποιήσεων push σε εφαρμογές τις ακόλουθες πλατφόρμες:

* Android
* iOS
* Windows Phone
* Πλατφόρμα γενικής χρήσης των Windows 

Για περισσότερες πληροφορίες σχετικά με την ειδοποίηση διανομείς, ανατρέξτε στην ενότητα [Επόμενα βήματα](#next) .

##<a name="what-are-notification-hubs"></a>Τι είναι οι διανομείς ειδοποίηση;

Azure ειδοποίηση διανομείς παρέχουν μια υποδομή εύκολο για χρήση πολλών πλατφόρμα, μεταβλητού μεγέθους για την αποστολή ειδοποιήσεων push σε κινητές συσκευές. Για λεπτομέρειες σχετικά με την υποδομή υπηρεσίας, ανατρέξτε στη σελίδα [Azure ειδοποίηση διανομείς](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Δημιουργία μιας εφαρμογής Node.js

Το πρώτο βήμα σε αυτό το πρόγραμμα εκμάθησης δημιουργεί μια νέα κενή εφαρμογή Node.js. Για οδηγίες σχετικά με τη δημιουργία μιας εφαρμογής Node.js, ανατρέξτε στο θέμα [Δημιουργία και ανάπτυξη μιας εφαρμογής Node.js σε τοποθεσία Web του Azure][nodejswebsite], [Μια υπηρεσία Cloud Node.js] [ Node.js Cloud Service] με τη χρήση του Windows PowerShell ή [την τοποθεσία Web με WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να χρησιμοποιήσετε διανομείς ειδοποίησης

Για να χρησιμοποιήσετε Azure διανομείς ειδοποίησης, πρέπει να κάνετε λήψη και χρήση του Node.js [azure πακέτου](https://www.npmjs.com/package/azure), η οποία περιλαμβάνει ένα ενσωματωμένο σύνολο βιβλιοθηκών Βοήθειας που επικοινωνία με τις υπηρεσίες ΥΠΌΛΟΙΠΑ ειδοποιήσεων push.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Χρήση διαχείρισης πακέτου κόμβου (NPM) για να αποκτήσετε το πακέτο

1.  Χρησιμοποιήστε ένα περιβάλλον γραμμής εντολών όπως **PowerShell** (Windows), το **Terminal** (Mac) ή το **πάρτι** (Linux) και μεταβείτε στο φάκελο όπου δημιουργήσατε την εφαρμογή του κενό.

2.  Πληκτρολογήστε **npm εγκατάσταση azure μικρές** στο παράθυρο εντολών.

3.  Μπορείτε να εκτελέσετε με μη αυτόματο τρόπο την εντολή **ls** ή **dir** για να επιβεβαιώσετε ότι ένα **κόμβου\_λειτουργικές μονάδες** που δημιουργήθηκε το φάκελο. Μέσα σε αυτόν το φάκελο, βρείτε το πακέτο **azure** , η οποία περιέχει τις βιβλιοθήκες που πρέπει να έχουν πρόσβαση την ενότητα "Ειδοποίηση".

>[AZURE.NOTE] Μπορείτε να μάθετε περισσότερα σχετικά με την εγκατάσταση του NPM σε το επίσημο [ιστολόγιο NPM](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>Εισαγωγή της λειτουργικής μονάδας

Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, προσθέστε τα ακόλουθα στο επάνω μέρος του αρχείου **server.js** της εφαρμογής:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Ρυθμίσετε μια σύνδεση διανομέα ειδοποίηση Azure

Το αντικείμενο **NotificationHubService** σάς επιτρέπει να εργάζεστε με ειδοποίηση διανομείς. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **NotificationHubService** για την ενότητα nofication με το όνομα **hubname**. Προσθέστε την κοντά στο επάνω μέρος του αρχείου **server.js** , μετά την πρόταση για να εισαγάγετε τη λειτουργική μονάδα azure:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Μπορείτε να αποκτήσετε την τιμή **συμβολοσειρά σύνδεσης** σύνδεσης από την [Πύλη Azure] , ακολουθώντας τα παρακάτω βήματα:

1. Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στο κουμπί **Αναζήτηση**.

2. Επιλέξτε **Διανομείς ειδοποιήσεων**και, στη συνέχεια, βρείτε την ενότητα που θέλετε να χρησιμοποιήσετε για το δείγμα. Μπορείτε να ανατρέξετε στην του [Windows Store ξεκινήσατε το πρόγραμμα εκμάθησης](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) , εάν χρειάζεστε βοήθεια για τη δημιουργία διανομέα νέα ειδοποίηση.

3. Επιλέξτε **Ρυθμίσεις**.

4. Κάντε κλικ στην επιλογή σχετικά με τις **πολιτικές πρόσβασης**. Θα δείτε δύο συμβολοσειρών σύνδεσης κοινόχρηστου και πλήρη πρόσβαση.

![Azure πύλης - διανομείς ειδοποίησης](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Μπορείτε επίσης να ανακτήσετε τη συμβολοσειρά σύνδεσης, χρησιμοποιώντας το cmdlet **Get-AzureSbNamespace** που παρέχεται από [Azure PowerShell](../powershell-install-configure.md) ή την εντολή **Εμφάνιση azure μικρές χώρο ονομάτων** με το [περιβάλλον γραμμής εντολών Azure (Azure CLI)](../xplat-cli-install.md).

##<a name="general-architecture"></a>Γενικές αρχιτεκτονικής

Το αντικείμενο **NotificationHubService** εκθέτει οι παρακάτω παρουσίες αντικειμένου για την αποστολή ειδοποιήσεων push για συγκεκριμένες συσκευές και τις εφαρμογές:

* **Android** - χρήση του αντικειμένου **GcmService** , το οποίο είναι διαθέσιμο στο **notificationHubService.gcm**
* **iOS** - χρήση του αντικειμένου **ApnsService** , το οποίο είναι δυνατή η πρόσβαση στο **notificationHubService.apns**
* **Windows Phone** - χρήση του αντικειμένου **MpnsService** , το οποίο είναι διαθέσιμο στο **notificationHubService.mpns**
* **Καθολική πλατφόρμα Windows** - χρήση του αντικειμένου **WnsService** , το οποίο είναι διαθέσιμο στο **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Πώς μπορείτε να: Αποστολή ειδοποιήσεων push για τις εφαρμογές του Android

Το αντικείμενο **GcmService** παρέχει μια μέθοδο **Αποστολή** που μπορούν να χρησιμοποιηθούν για την αποστολή ειδοποιήσεων push για τις εφαρμογές του Android. Η μέθοδος **Αποστολή** δέχεται τις ακόλουθες παραμέτρους:

* **Ετικέτες** - το αναγνωριστικό ετικέτας. Εάν δεν υπάρχει η ετικέτα παρέχεται, την ειδοποίηση θα σταλεί στα προγράμματα-πελάτες al.
* **Φορτίο** - JSON ή φορτίο ανεπεξέργαστα συμβολοσειρά του μηνύματος.
* **Επιστροφή κλήσης** - η λειτουργία επιστροφής κλήσης.

Για περισσότερες πληροφορίες σχετικά με τη μορφή φορτίο, ανατρέξτε στην ενότητα **φορτίο** του εγγράφου του [Διακομιστή GCM εφαρμογής](http://developer.android.com/google/gcm/server.html#payload) .

Ο ακόλουθος κώδικας χρησιμοποιεί την παρουσία **GcmService** που εκτίθεται από την **NotificationHubService** για να στείλετε μια ειδοποίηση push σε όλα τα καταχωρημένα προγράμματα-πελάτες.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Πώς μπορείτε να: Αποστολή ειδοποιήσεων push για τις εφαρμογές του iOS

Ίδια όπως και με Android εφαρμογές που περιγράφεται παραπάνω, το αντικείμενο **ApnsService** παρέχει μια μέθοδο **Αποστολή** που μπορούν να χρησιμοποιηθούν για την αποστολή ειδοποιήσεων push για τις εφαρμογές του iOS. Η μέθοδος **Αποστολή** δέχεται τις ακόλουθες παραμέτρους:

* **Ετικέτες** - το αναγνωριστικό ετικέτας. Εάν δεν υπάρχει η ετικέτα παρέχεται, η ειδοποίηση θα σταλεί σε όλους τους πελάτες.
* **Φορτίο** - JSON του μηνύματος ή φορτίου συμβολοσειρά.
* **Επιστροφή κλήσης** - η λειτουργία επιστροφής κλήσης.

Για περισσότερες πληροφορίες σχετικά με τη μορφή φορτίο, ανατρέξτε στην ενότητα **Φορτίο ειδοποίηση** του [τοπικού και Οδηγό προγραμματισμού ειδοποιήσεων Push](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) εγγράφου.

Ο ακόλουθος κώδικας χρησιμοποιεί την παρουσία **ApnsService** που εκτίθεται από την **NotificationHubService** για να στείλετε ένα μήνυμα προειδοποίησης σε όλους τους πελάτες:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Πώς μπορείτε να: Αποστολή ειδοποιήσεων push για τις εφαρμογές του Windows Phone

Το αντικείμενο **MpnsService** παρέχει μια μέθοδο **Αποστολή** που μπορούν να χρησιμοποιηθούν για την αποστολή ειδοποιήσεων push για τις εφαρμογές του Windows Phone. Η μέθοδος **Αποστολή** δέχεται τις ακόλουθες παραμέτρους:

* **Ετικέτες** - το αναγνωριστικό ετικέτας. Εάν δεν υπάρχει η ετικέτα παρέχεται, η ειδοποίηση θα σταλεί σε όλους τους πελάτες.
* **Φορτίο** - φορτίο XML του μηνύματος.
* **Όνομα προορισμού**  -  `toast` για τις ειδοποιήσεις αναδυόμενη. `token`για ειδοποιήσεις δυναμικών πλακιδίων.
* **NotificationClass** - την προτεραιότητα της ειδοποίησης. Ανατρέξτε στην ενότητα **Στοιχεία κεφαλίδας HTTP** του εγγράφου [ειδοποιήσεις από ένα διακομιστή Push](http://msdn.microsoft.com/library/hh221551.aspx) για τις έγκυρες τιμές.
* **Επιλογές** - κεφαλίδες προαιρετικό αίτησης.
* **Επιστροφή κλήσης** - η λειτουργία επιστροφής κλήσης.

Για μια λίστα των επιλογών έγκυρο **όνομα προορισμού**, **NotificationClass** και κεφαλίδας, δείτε τη σελίδα [ειδοποιήσεις από ένα διακομιστή προώθησης](http://msdn.microsoft.com/library/hh221551.aspx) .

Το παρακάτω δείγμα κώδικα χρησιμοποιεί την παρουσία **MpnsService** που εκτίθεται από την **NotificationHubService** για να στείλετε μια αναδυόμενη ειδοποίηση push:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Πώς μπορείτε να: Αποστολή ειδοποιήσεων push σε εφαρμογές καθολικής πλατφόρμας των Windows (UWP)

Το αντικείμενο **WnsService** παρέχει μια μέθοδο **Αποστολή** που μπορούν να χρησιμοποιηθούν για την αποστολή ειδοποιήσεων push σε εφαρμογές καθολικής πλατφόρμας των Windows.  Η μέθοδος **Αποστολή** δέχεται τις ακόλουθες παραμέτρους:

* **Ετικέτες** - το αναγνωριστικό ετικέτας. Εάν δεν υπάρχει η ετικέτα παρέχεται, την ειδοποίηση θα σταλεί σε όλα τα καταχωρημένα προγράμματα-πελάτες.
* **Φορτίο** - το φορτίο μήνυμα XML.
* **Τύπος** - τον τύπο ειδοποίησης.
* **Επιλογές** - κεφαλίδες προαιρετικό αίτησης.
* **Επιστροφή κλήσης** - η λειτουργία επιστροφής κλήσης.

Για μια λίστα των τύπων έγκυρη και κεφαλίδες αίτησης, ανατρέξτε στο θέμα [κεφαλίδες αίτησης και απόκρισης υπηρεσία ειδοποιήσεων Push](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Ο ακόλουθος κώδικας χρησιμοποιεί την παρουσία **WnsService** που εκτίθεται από την **NotificationHubService** για να στείλετε μια αναδυόμενη ειδοποίηση push σε μια εφαρμογή UWP:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Επόμενα βήματα

Το παραπάνω παράδειγμα τμήματα σάς επιτρέπουν να δημιουργήσετε εύκολα υποδομή υπηρεσία για την παράδοση ειδοποιήσεων push σε μια μεγάλη ποικιλία συσκευών. Τώρα που μάθατε τα βασικά στοιχεία της χρήσης διανομείς ειδοποίηση με node.js, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με το πώς μπορείτε να επεκτείνετε περαιτέρω αυτές τις δυνατότητες.

-   Ανατρέξτε στο άρθρο αναφορά MSDN για [διανομείς Azure ειδοποίησης](https://msdn.microsoft.com/library/azure/jj927170.aspx).
-   Επισκεφθείτε το αποθετήριο [Azure SDK για κόμβο] στο GitHub για περισσότερα δείγματα και λεπτομέρειες υλοποίησης.

  [Azure SDK για κόμβου]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Τοποθεσία Web με WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Πύλη του Azure]: https://portal.azure.com
